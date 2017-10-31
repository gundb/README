<p>
    <a href="http://gun.js.org/"><img width="40%" src="https://cldup.com/TEy9yGh45l.svg"/></a>
    <img width="50%" align="right" vspace="25" src="http://gun.js.org/see/demo.gif"/>
</p>

[![npm](https://img.shields.io/npm/dm/gun.svg)](https://www.npmjs.com/package/gun)
[![Travis](https://img.shields.io/travis/amark/gun/master.svg)](https://travis-ci.org/amark/gun)
[![FOSSA Status](https://app.fossa.io/api/projects/git%2Bhttps%3A%2F%2Fgithub.com%2Famark%2Fgun.svg?size=shield)](https://app.fossa.io/projects/git%2Bhttps%3A%2F%2Fgithub.com%2Famark%2Fgun?ref=badge_shield)
[![Gitter](https://img.shields.io/gitter/room/amark/gun.js.svg)](https://gitter.im/amark/gun)

GUN is a realtime, distributed, offline-first, graph database engine. Doing **[20M+](https://github.com/amark/gun/wiki/100000-ops-sec-in-IE6-on-2GB-Atom-CPU) ops/sec** in just **~9KB** gzipped.

<a href="https://youtu.be/-i-11T5ZI9o" title="1 minute demo of fault tolerance"><img src="http://img.youtube.com/vi/-i-11T5ZI9o/0.jpg" width="425px"></a>
<a href="https://youtu.be/-FN_J3etdvY" title="1 minute demo of fault tolerance"><img src="http://img.youtube.com/vi/-FN_J3etdvY/0.jpg" width="425px"></a>

## Why?

 - **Realtime** - You might use socketio for realtime updates, but what happens if you reload the page? GUN solves *state synchronization* for you, no matter what, on reloads, across all your users, and even on conflicting updates.
 - **Distributed** - GUN is peer-to-peer by design, meaning you have no centralized database server to maintain or that could crash. This lets you sleep through the night without worrying about database DevOps - we call it "NoDB". From there, you can build decentralized, federated, or centralized apps.
 - **Offline-first** - GUN works even if your internet or cell reception doesn't. Users can still plug away and save data as normal, and then when the network comes back online GUN will automatically synchronize all the changes and handle any conflicts for you.
 - **Graph** - Most databases force you to bend over backwards to match their storage constraints. But graphs are different, they let you have any data structure you want. Whether that be traditional tables with relations, document oriented trees, or tons of circular references. You choose.

## Quickstart

 - Try the [interactive tutorial](http://gun.js.org/think.html) in the browser (**5min** ~ average developer).
 - Or `npm install gun` and run the examples with `cd node_modules/gun && npm start` (**5min** ~ average developer).

> **Note:** If you don't have [node](http://nodejs.org/) or [npm](https://www.npmjs.com/), read [this](https://github.com/amark/gun/blob/master/examples/install.sh) first.
> If the `npm` command line didn't work, you may need to `mkdir node_modules` first or use `sudo`.

- An online demo of the examples are available here: http://gunjs.herokuapp.com/
- Or write a quick app: ([try now in jsbin](http://jsbin.com/sovihaveso/edit?js,console))
```html
<script src="https://cdn.jsdelivr.net/npm/gun/gun.js"></script>
<script>
// var Gun = require('gun'); // in NodeJS
// var Gun = require('gun/gun'); // in React
var gun = Gun();

gun.get('mark').put({
  name: "Mark",
  email: "mark@gunDB.io",
});

gun.get('mark').on(function(data, key){
  console.log("update:", data);
});
</script>
```
- Or try something mind blowing, like saving circular references to a table of documents! ([play](http://jsbin.com/wefozepume/edit?js,console))
```javascript
var cat = {name: "Fluffy", species: "kitty"};
var mark = {boss: cat};
cat.slave = mark;

// partial updates merge with existing data!
gun.get('mark').put(mark);

// access the data as if it is a document.
gun.get('mark').get('boss').get('name').val(function(data, key){
  // `val` grabs the data once, no subscriptions.
  console.log("Mark's boss is", data);
});

// traverse a graph of circular references!
gun.get('mark').get('boss').get('slave').val(function(data, key){
  console.log("Mark is the slave!", data);
});

// add both of them to a table!
gun.get('list').set(gun.get('mark').get('boss'));
gun.get('list').set(gun.get('mark'));

// grab each item once from the table, continuously:
gun.get('list').map().val(function(data, key){
  console.log("Item:", data);
});

// live update the table!
gun.get('list').set({type: "cucumber", goal: "scare cat"});
```

## Support

<p align="center">
Thanks to:<br/>
<a href="http://qxip.net/"><img src="http://qxip.net/images/qxip_logo.png" width="100"></a><br/>
<a href="http://github.com/samliu">Sam Liu</a>, 
<a href="http://github.com/ddombrow">Daniel Dombrowsky</a>, 
<a href="http://github.com/vincentwoo">Vincent Woo</a>, 
<a href="http://github.com/coolaj86">AJ ONeal</a>, 
<a href="http://github.com/ottman">Bill Ottman</a>,
<a href="http://github.com/ctrlplusb">Sean Matheson</a>, 
<a href="http://github.com/alanmimms">Alan Mimms</a>
</p>

 - Join others in sponsoring code: https://www.patreon.com/gunDB !
 - Ask questions: http://stackoverflow.com/questions/tagged/gun ?
 - Found a bug? Report at: https://github.com/amark/gun/issues ;
 - **Need help**? Chat with us: https://gitter.im/amark/gun .


## Documentation

<table>
  <tr>
    <td style="border: 0;"><h3><a href="https://github.com/amark/gun/wiki/API">API reference</a></h3></td>
    <td style="border: 0;"><h3><a href="http://gun.js.org/think.html">Tutorials</a></h3></td>
    <td style="border: 0;"><h3><a href="https://github.com/amark/gun/tree/master/examples">Examples</a></h3></td>
  </tr>
  <tr>
    <td style="border: 0;"><h3><a href="https://github.com/brysgo/graphql-gun">GraphQL</a></h3></td>
    <td style="border: 0;"><h3><a href="https://github.com/PenguinMan98/electrontest">Electron</a></h3></td>
    <td style="border: 0;"><h3><a href="https://github.com/staltz/gun-asyncstorage">Native</a></h3></td>
  </tr>
  <tr>
    <td style="border: 0;"><h3><a href="https://github.com/sjones6/vue-gun">Vue</a></h3></td>
    <td style="border: 0;"><h3><a href="https://github.com/amark/gun/wiki/React-Tutorial">React</a></h3></td>
    <td style="border: 0;"><h3><a href="https://github.com/Stefdv/gun-ui-lcd#syncing">Webcomponents</a></h3></td>
  </tr>
  <tr>
    <td style="border: 0;"><h3><a href="https://github.com/amark/gun/wiki/CAP-Theorem">CAP Theorem Tradeoffs</a></h3></td>
    <td style="border: 0;"><h3><a href="http://gun.js.org/distributed/matters.html">How Data Sync Works</a></h3></td>
    <td style="border: 0;"><h3><a href="https://github.com/amark/gun/wiki/porting-gun">How GUN is Built</a></h3></td>
  </tr>
  <tr>
    <td style="border: 0;"><h3><a href="https://github.com/amark/gun/wiki/auth">Crypto Auth</a></h3></td>
    <td style="border: 0;"><h3><a href="https://github.com/amark/gun/wiki/Modules">Modules</a></h3></td>
    <td style="border: 0;"><h3><a href="https://github.com/amark/gun/wiki/roadmap">Roadmap</a></h3></td>
  </tr>
</table>

This would not be possible without **community contributors**, big shout out to:

**[BrockAtkinson](https://github.com/BrockAtkinson) ([brunch config](https://github.com/BrockAtkinson/brunch-gun))**; **[Brysgo](https://github.com/brysgo) ([GraphQL](https://github.com/brysgo/graphql-gun))**; **[d3x0r](https://github.com/d3x0r) ([SQLite](https://github.com/d3x0r/gun-db))**; **[forrestjt](https://github.com/forrestjt) ([file.js](https://github.com/amark/gun/blob/master/lib/file.js))**; **[JosePedroDias](https://github.com/josepedrodias) ([graph visualizer](http://acor.sl.pt:9966))**; **[JuniperChicago](https://github.com/JuniperChicago) ([cycle.js bindings](https://github.com/JuniperChicago/cycle-gun))**; **[jveres](https://github.com/jveres) ([todoMVC](https://github.com/jveres/todomvc))**; **[kristianmandrup](https://github.com/kristianmandrup) ([edge](https://github.com/kristianmandrup/gun-edge))**; [PsychoLlama](https://github.com/PsychoLlama) ([LevelDB](https://github.com/PsychoLlama/gun-level)); **[RangerMauve](https://github.com/RangerMauve) ([schema](https://github.com/gundb/gun-schema))**; **[robertheessels](https://github.com/swifty) ([gun-p2p-auth](https://github.com/swifty/gun-p2p-auth))**; [sbeleidy](https://github.com/sbeleidy); **[Sean Matheson](https://github.com/ctrlplusb) ([Observable/RxJS/Most.js bindings](https://github.com/ctrlplusb/gun-most))**; **[Stefdv](https://github.com/stefdv) (Polymer/web components)**; **[sjones6](https://github.com/sjones6) ([Flint](https://github.com/sjones6/gun-flint))**;

I am missing many others, apologies, will be adding them soon!

## Deploy

To quickly spin up a Gun test server for your development team, utilize either [Heroku](http://heroku.com) or [Docker](http://docker.com) or any variant thereof [Dokku](http://dokku.viewdocs.io/dokku/), [Flynn.io](http://flynn.io), [now.sh](https://zeit.co/now), etc. !

### [Heroku](https://www.heroku.com/)

[![Deploy](https://www.herokucdn.com/deploy/button.svg)](https://heroku.com/deploy?template=https://github.com/amark/gun)

Or:

```bash
git clone https://github.com/amark/gun.git
cd gun
heroku create
git push -f heroku HEAD:master
```

Then visit the URL in the output of the 'heroku create' step, in a browser.

### [Now.sh](https://zeit.co/now/)

```bash
npm install -g now
now --npm amark/gun
```

Then visit the URL in the output of the 'now --npm' step, in your browser.

### [Docker](https://www.docker.com/)

[![Docker Automated buil](https://img.shields.io/docker/automated/gundb/gun.svg)](https://hub.docker.com/r/gundb/gun/) [![](https://images.microbadger.com/badges/image/gundb/gun.svg)](https://microbadger.com/images/gundb/gun "Get your own image badge on microbadger.com") [![Docker Pulls](https://img.shields.io/docker/pulls/gundb/gun.svg)](https://hub.docker.com/r/gundb/gun/) [![Docker Stars](https://img.shields.io/docker/stars/gundb/gun.svg)](https://hub.docker.com/r/gundb/gun/)

Pull from the [Docker Hub](https://hub.docker.com/r/gundb/gun/) [![](https://images.microbadger.com/badges/commit/gundb/gun.svg)](https://microbadger.com/images/gundb/gun). Or:

```bash
docker run -p 8080:8080 gundb/gun
```

Or build the [Docker](https://docs.docker.com/engine/installation/) image locally:

```bash
git clone https://github.com/amark/gun.git
cd gun
docker build -t myrepo/gundb:v1 .
docker run -p 8080:8080 myrepo/gundb:v1
```

Or, if you prefer your Docker image with metadata labels (Linux/Mac only):

```bash
npm run docker
docker run -p 8080:8080 usenameHere/gun:git
```

Then visit [http://localhost:8080](http://localhost:8080) in your browser.

## License

Designed with ♥ by Mark Nadal, the GUN team, and many amazing contributors.

Openly licensed under [Zlib / MIT / Apache 2.0](https://github.com/amark/gun/blob/master/LICENSE.md).

[![FOSSA Status](https://app.fossa.io/api/projects/git%2Bhttps%3A%2F%2Fgithub.com%2Famark%2Fgun.svg?size=large)](https://app.fossa.io/projects/git%2Bhttps%3A%2F%2Fgithub.com%2Famark%2Fgun?ref=badge_large)

[YouTube](https://www.youtube.com/channel/UCQAtpf-zi9Pp4__2nToOM8g) . [Twitter](https://twitter.com/marknadal)
