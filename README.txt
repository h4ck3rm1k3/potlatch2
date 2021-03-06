== Halcyon and Potlatch 2.0 - ActionScript 3 renderer and editor ==

Potlatch 2.0 is the new version of the OpenStreetMap online editor.

Halcyon is its rendering engine. It's rules-based (like, say, Mapnik) and does dotted lines, text on a path, casing, icons for POIs, all of that.

Both are written in ActionScript 3. Potlatch 2.0 additionally uses the Flex framework.

Many icons used in halcyon/potlatch2 are based on the awesome CC0-licensed SJJB icons project. http://www.sjjb.co.uk/mapicons/

=== What you'll need ===

* Flex SDK 4.5
** Product page: http://www.adobe.com/products/flex/ 
** Flex download page: http://www.adobe.com/devnet/flex/flex-sdk-download-all.html (free, OS X/Windows/Linux)
* AS3 docs - http://livedocs.adobe.com/flash/9.0/ActionScriptLangRefV3/
* Flash debug player - http://www.adobe.com/support/flashplayer/downloads.html
* Basically you might as well just sell your soul to Adobe
* Ant

If you happen to have Adobe Flex Builder 3/Flash Builder 4, you can create a project and import files into it. 
See http://wiki.openstreetmap.org/wiki/Potlatch_2/Developer_Documentation for details.

You'll only need OSM Rails port installed on your local machine if you are doing hard-core 
server-communication coding, but if generally you can use the dev server at api06.dev.openstreetmap.org
for development and testing.


=== How to compile and run ===

Compiling Potlatch 2:

1) Copy the properties template file
  cp build.properties.template build.properties
  
2) Edit the FLEX_HOME variable in build.properties  
 eg, FLEX_HOME=c:/flex_sdk/4.5.0.20967
 
3) ant

The following command will compile potlatch2 in debug configuration
The result is put at resources/potlatch2.swf

* ant

The following command will compile potlatch2 in release configuration

* ant release

Compiling Halcyon as standalone viewer:

* ant halcyon

You can create class documentation (in resources/docs) using asdoc

* ant docs

You can create and run the unit tests (not that there are that many) using flexunit

* ant test

For those that don't need I8n, the following give a much speedier build as it skips the all the language translation build steps.
As an extra bonus, this uses much less memory and you may get away without needing to tell ant to use more memory (see below).

* ant debug-no-locales
or
* ant release-no-locales

If you're using Mac OS X, you may need to tell ant to use more memory, by
typing export ANT_OPTS="-Xms768m -Xmx1024m -XX:MaxPermSize=512m" 
beforehand (you can put this in your .profile).


Compiling during development:

Compiling optimized versions from scratch takes a _long_ time. There are
several ways to make it faster during development and also add useful
debug stack traces and enable the commandline debugger (at the expense
of a much larger swf file.. but we're developing so that doesn't matter!).

* fcsh
  - launches the Flex Compiler SHell -- stops the compiler having to
    bootstrap itself each time you invoke it. You don't /need/ this, but it
    does make things slightly faster (about a second a shot for me)

* mxmlc -load-config+=debug-config.xml potlatch2.mxml
  - compile potlatch2 in debug configuration -- build is incremental so you
    can run it again and mxmlc will only compile changes. Output has debug
    enabled along with decent stack traces.
    (you can substitute halcyon_viewer.as in the above to compile that)

* compile 1 
  - when using fcsh recompile the first command

* compile 1
  - type it again to compile again. You'll really wish that up-arrow,enter 
    worked, but Adobe is laughing at you RIGHT NOW.

* rlwrap
  - if you have it on your system (e.g. linux), rlwrap is a godsend. Launch
    fcsh with 'rlwrap path/to/fcsh' and up arrows will work, even persistantly
    across one fcsh session to the next.

Running:

* Flash security model sucks. If you want to use internet resource (e.g. map calls to the dev
  server) the binary must have been served from "teh internets". Run resources/server.py to launch a local
  server, then go to http://localhost:3333/potlatch2.html to get started (or if you're already running e.g. 
  Apache locally, feel free to use that instead.)
  
  Alternatively, you can update your global Flash security settings to "always trust files" in your local dev area:
  http://www.macromedia.com/support/documentation/en/flashplayer/help/settings_manager04.html
  Then you can run the swf file directly.
  
* If you are doing offline development, you will need a rails_port install. You 
  will need to add an OAuth application by going to 
  http://rails-port.local/user/<username>/oauth_clients/new
  Enter the following details (assuming the above point):
  * Name (Required): Potlatch2 (local)
  * Main Application URL (Required): http://localhost:3333/resources/potlatch2.html
  And then update resources/potlatch2.html replacing the domains.

=== Some other stuff you might need to know ===

* Flex compiler runs at about the speed of a tortoise soaked in molasses which happens also to be dead.
* Running the debug player helps when coding, since it'll pop up the runtime errors. You don't see them
  with the normal player.
  
Richard Fairhurst
richard@systemeD.net

Dave Stubbs
osm@randomjunk.co.uk

