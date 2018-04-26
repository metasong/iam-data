# ng-conf
here are all the slides and talks 
http://confsnap.com/event/ng-conf-18

[‎Thu,‎4/‎26/‎2018 11:09 AM]  Brandt Beal:  
No Title 
Some of the more interesting topics are NRWL NX, Bazel, RxJs 6, Elements, custom build, firebase ml functions, updates to protractor and selenium 4, and last but not least project ivy.  Project ivy is the new renderer and compiler that fixes a lot of the cryptic errors we get in templates and will support source maps in the templates themselves.  You can actually set a break point in the angular template and it will break on the compiled rendering function; pretty nice.  The event focused on enterprise development, improvements to rendering speed to support angular elements, tree shakable everything and schematics.  They demonstrated several tools that can do some advanced transformations like upgrade your entire code based from rxjs 5 import by side effect to rxjs 6 pipeables and upgrading an angular 1 app to angular 2 without any intervention.  The only way this can happen though is if you properly type your code.  “Any” types can’t be interpreted or inferred and shouldn’t be used if you want support from any of these tools.  Also closure compiler can’t do it’s job if the type annotations aren’t present.  So they recommend tightening up those reigns if you haven’t already. 
 

