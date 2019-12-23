# SceneLoader.gd for the Godot Game Engine
This script provides an easy way to asynchronously load in assets or scenes at runtime without the usual hiccup!
More details are written in the script.

DiSCLAIMER: Does not work on web, since Threads are not supported

# Installation
Download and extract the .zip-File and drag "SceneLoader.gd" into your project. (You can also download this addon on the Godot Asset library)
Create a new Autoload with "SceneLoader.gd" in Project > Settings > (Tab) Autoload, set the Path and click the ADD-Button.

# Usage
Once the script has been set as AutoLoad, you will be able to call SceneManager.load_scene("...") from any Script-File.

* To queue a scene or asset for loading, type:

`SceneLoader.load_scene("res://my_scene.tscn", { prop1 = "Hi!" })`

* To be notified about the end of the scene loading, add this to your _ready() function:

```javascript
func _ready():
    SceneLoader.connect("on_scene_loaded", self, "do_scene_loaded")

func do_scene_loaded(scene):
    # Do something with the scene
    pass
```
* The SceneLoader will pass an object with following structure to your do_scene_loaded function:

```javascript
{ 
    path = path, 
    loader = (ResourceInteractiveLoader), 
    instance = (your scene instance), 
    props = props (Properties you passed with the load_scene call) 
}
```

* You can now add the scene to the scene tree, examine the props and filter by path:

```javascript
if scene.path == "res://my_scene.tscn":
    print(scene.props.prop1)
    add_child(scene.instance)
```

# Additional info

SceneLoader.gd loads a scene in the background using a seperate thread and a queue.
Foreach new scene there will be an instance of ResourceInteractiveLoader
that will raise an on_scene_loaded event once the scene has been loaded.
Hooking the on_progress event will give you the current progress of any
scene that is being processed in realtime. The loader also uses caching
to avoid duplicate loading of scenes and it will prevent loading the
same scene multiple times concurrently.