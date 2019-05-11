---
layout: post
title: API Facade with RxJS
tags:
  - angular
  - rxjs
  - facade
author: Dan Smith
date: '2019-05-11'
---

Last week I attended ngConf 2019 in Salt Lake City and there were two talks in particular that stood out to me. One was [this workshop](https://www.youtube.com/watch?v=XKfhGntZROQ) by [Michael Hladkey](https://twitter.com/Michael_Hladky) on highly dynamic UIs driven by RxJS. The other was [this talk](https://www.youtube.com/watch?v=h-F5uYM69a4) given by [Thomas Burleson](https://twitter.com/ThomasBurleson) on using the Facade pattern to handle state management (without NgRX). My idea was to combine these two to drive a page connected to an API.

First I put together a component implementing it in the style that I normally would, where the component itself handles the API calls, paging, and searching. You can see that at [this StackBlitz](https://stackblitz.com/edit/api-paging-example?embed=1&file=src/app/api-search/api-search-page/api-search-page.component.ts). The component code looks something like this:
```typescript
export class ApiSearchPageComponent implements OnInit {
  public searchInput = new FormControl();
  public articles: any[] = [];
  public totalHits: number;
  public page: number = 0;
  public searchTerm: string;
  
  constructor(
    private api: ApiService,
    private http: HttpClient
  ) {}
  
  public ngOnInit() {
    this.getArticles();
    this.searchInput.valueChanges
      .pipe(
        debounceTime(1_000),
        distinctUntilChanged()
      ).subscribe((searchTerm) => {
        this.page = 0;
        this.searchTerm = searchTerm;
        this.getArticles(searchTerm);
      })
  }

  public getArticles(searchTerm = '') {
    this.api.getArticles({ query: searchTerm, page: this.page })
      .subscribe((resp) => {
        this.totalHits = resp.nbHits;
        this.articles = resp.hits;
      });
  }

  public changePage(event: PageEvent) {
    this.page = event.pageIndex;
    this.getArticles(this.searchTerm);
  }
}
```

After that I took a stab at implementing a Facade to handle the API calls. The component code is now reduced to this:
```typescript
export class ApiSearchPageComponent implements OnInit {
  public searchInput = new FormControl();
  
  constructor(
    public api: ApiFacade
  ) {}
  
  public ngOnInit() {
    this.searchInput.valueChanges
      .subscribe((searchTerm) => {
        this.api.updateSearchTerm(searchTerm);
      });
  }

  public changePage(event: PageEvent) {
    this.api.updatePage(event.pageIndex);
    this.api.updatePerPage(event.pageSize);
  }
}
```
You can see the full blitz [here](https://stackblitz.com/edit/api-paging-example-with-facade?embed=1&file=src/app/api-search/api-search-page/api-search-page.component.ts).

The first thing I noticed while doing this was how much logic I was able to move out of the component and into the facade service. The second thing I noticed was how much more code there is in the facade/RxJS implementation. This is one of the tradeoffs with this pattern, but the benefit is we hide the complexity of working with the API from the component. So if someone needs to make changes to the component in the future, it should be simpler to do and should be able to make those changes with more confidence since the logic behind fetching data from the API is hidden in the facade.

One of the benefits of the facade pattern I noticed was if we need to move this into NgRX at some point, we can probably do that without having to change the component since the implementation is hidden behind the facade. We should still be able to use the facade service to deliver the same observables and update actions to the component using NgRX.

It took me a bit to figure out the right combination of RxJS Observables/operators to make this work how I wanted, and I'm sure there's still room for improvement, but for now it works with purely reactive programming with no state being stored outside the streams.
