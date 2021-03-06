---
layout: post
title: 'Ember and the Mystery of an Efficient Nested Index '
date: '2014-05-30T09:57:00.000-04:00'
author: Daniel Smith
tags:
- ruby
- ember
- rails
- nested resource
---

While trying to port parts of a large application to Ember, I kept running into issues with how to handle nested resources efficiently. Assuming you have a Post model which hasMany Comments, it seems the built-in, conventional way to handle the nested comments is to get the Post object then nest either all the ids or the objects themselves inside the root object. But then when trying to call the nested resource, by default just a base /comments call is made to the API.<br /><br />This could quickly get out of hand if you have thousands or tens of thousands of comments, or if the model you are trying to call is more complex than a typical post comment. Ideally there would be a simple way to call /posts/1/comments and just return the comments for that post. And there is a way to do this, but it took me quite a while to nail it down properly.<br /><br />The key is to use a 'links' element on the root object. The idea is you will load the root object, then tell the REST adapter (or other data layer) where to go to get the resources there. <a href="http://stackoverflow.com/questions/21714228/loading-ember-data-hasmany-relationships-using-links" target="_blank">This SO link</a> shows conceptually how it should work from Ember (you could also conceivably set this up on the server side if you have access). That part is fairly straight forward, and once set up you can test in console by typing:

```javascript
post = App.Post.store.find('post', 1);
post.get('comments');
```

Once you verify that the call actually does work, the next step is to get the CommentsRoute to call this as it's model. The key here is the modelFor method. Here is the code that finally worked for me:

```javascript
App.CommentsRoute = Ember.Route.extend({
  model: function() {
    return this.modelFor('post').get('comments');
  }
});
```

This checks to see who the parent is for the CommentsRoute is, grabs the model, then gets the comments attribute. When it sees the attribute is asynchronous and has a link, it grabs the link and uses that to populate the model.
