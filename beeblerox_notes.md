Beeblerox's original notes:

**What can be improved in flixel without breaking backward compatibility:**
  - Better support for sloped tiles (use SAT for this?)
  - Finish shaders support for individual sprites on native targets (it's kinda ready, but needs some checks).
  - Add new renderers: stage3d (in progress), OpenGL/WebGL?
  - Add support for multipage texture atlases.
  - Support for polygon sprite textures exported from TexturePacker (through FlxStrip class).
  - Add boundingBox property to FlxStrip, which will decrease calculations necessary for visibility checks, etc.
  - FlxStrip graphic caching in blit mode for better perfomance when graphic isn't changing often (just try to check if this is possible).
  - Maybe expand animation system, so it will be possible to have ping-pong animations, set delays for individual frames in animation, etc.
  - Add new graphic classes, like rope from pixi.js?
  - Fix some of existing bugs (for example, ).
   
**What can be implemented but will mean breaking changes:**
  - Change `origin` behavior in FlxSprite, especially for scaled sprites. Current behavior isn't easy to understand and seems strange to new users. But add new properties, like `top`, `bottom`, `left`, `right`, which will help positioning sprites on the screen.
  - Add content scale factor?
  - Heavy engine refactoring, making it use less inheritance preferring composition:
    - Add Transform component to FlxObject, something like Unity3D. Transform component will hold information about object's position, rotation, scale, etc. Plus it will have `parent` property, which will allow to have some kind of Display list (better than current FlxNestedSprite).
    - Moving rendering logic out of FlxSprite into FlxImage (FlxGraphic) class, like in HaxePunk. Decouple visuals from logic.
    - Moving physics logic out of FlxObject into separate class. This will make easier to have (add) custom physics.
    - Both physics and graphics objects will use tranformation info from FlxObject's Tranform component.
    - Add list of components to each FlxObject, which will allow to make game object's logic more modular (but not necessary).
    - Add `state` property to FlxObjects, this change is necesary for substate ui interaction (there is no checks in ui classes if the parent state is active/inactive).
    - Add `FlxCanvas` class which will use render textures (where they are available). This will be useful for ui-related graphic classes, and won't add much perfomance overhead.
    - Add tranformation caching, so there would be less calculations each frame.
    - Add `surface` optional argument to `draw()` methods, so we will be able to render object not only on camera, but on other objects as well.
    - Add state management class, which will make it possible to run multiple states at the same time, allow them not to be automatically destroyed on state change.
    - 2 level assets caching: one level for the whole game (persist through state switches) and one for state. Of course there will be some sort of "SuperDuperFlxSprite" which will behave much like FlxSprite today (for easier transition to new version of engine).

