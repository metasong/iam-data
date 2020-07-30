# RX Join operator
---
author: Jason Song <metaseed@gmail.com>
version: 1.0.0
tag: []
enable: [toc]
---

```csharp
#r "nuget:System.Reactive.Linq/4.4.1"
using System;
using System.Reactive;
using System.Reactive.Linq;


var cmd = Observable
.Interval(TimeSpan.FromMilliseconds(100))
.Select(id=>new  {Id = id, Des="cmd" + id});

var cmdStatus = Observable
.Interval(TimeSpan.FromMilliseconds(500))
.Select(id=>new {Id = id, Des="cmdstatus"+id});

//var r = cmdStatus.Join(cmd,  status => Observable.Empty<Unit>(),  cmd => Observable.Timer(TimeSpan.FromMilliseconds(400)),(status, cmd) => new {cmd, status} )
var r = cmdStatus.Join(cmd,  status => Observable.Timer(TimeSpan.FromMilliseconds(125)),  cmd => Observable.Timer(TimeSpan.FromMilliseconds(400)),(status, cmd) => new {cmd, status} )
//var r = cmdStatus.Join(cmd,  status => Observable.Timer(TimeSpan.FromMilliseconds(400)),  cmd => Observable.Timer(TimeSpan.FromMilliseconds(400)),(status, cmd) => new {cmd, status} )
//var r = cmdStatus.Join(cmd,cmd => Observable.Empty<Unit>(),  status => Observable.Never<Unit>(),  (status, cmd) => new {cmd, status} )

.Take(40);
r.Subscribe(x => Console.WriteLine(x.cmd.Des + x.status.Des));
while (true) {Thread.Sleep(5);}
```

