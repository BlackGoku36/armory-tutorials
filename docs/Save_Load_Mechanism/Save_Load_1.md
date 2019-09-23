## Part I
In this part we will implement basic of file reading and writing from in-game. At the end of this part you should be able to save/write json to `Bundled` and then load/read json from `Bundled` both during in-game.

---

Let create a Haxe trait(`SaveLoadMechanism`) for handling our save and load mechanism.

Now, for learning sake, we will be using [json](https://en.wikipedia.org/wiki/JSON) to store and read information from, later you can do stuff to make it unreadable to us humans to prevent cheating. [Haxe](https://haxe.org/) provide really good support for json and xml parsing and writing, making it easier for us to use.

```haxe
// In SaveLoadMechanism.hx

package arm;

class SaveLoadMechanism extends iron.Trait {
    public function new() {
        super();

        notifyOnInit(function() {
            //This is platform specific block, you can define 
            //different behaviour for different platforms.
            #if kha_krom
            //Set json structure with text as hello World!
            var saveData = { text: "Hello World!" };
            //Converts above structure to json string.
            var saveDataJSON = haxe.Json.stringify(saveData);

            //Get Krom's location path and add path for save_game.json.
            //This will be saved at root_folder/build_file/debug/krom/save_game.json.
            var path = Krom.getFilesLocation() + "/save_game.json";

            //Write json string to bytes.
            var bytes = haxe.io.Bytes.ofString(saveDataJSON);
            //Save to file from path specified above with data from bytes.
            Krom.fileSaveBytes(path, bytes.getData());
            #end
        });

    }
}
```

Now, if you play the game and go over to `root_folder/build_file/debug/krom/` you should find `save_game.json` and on opening it should read `{"text":"Hello World!"}`, if you do, then Congratulation! You did it!

Let save `save_game.json` to proper place and add some keyboard input code to handle saving manualy instead of saving when the game initiate.

```haxe
// In SaveLoadMechanism.hx

package arm;

import iron.system.Input;

class SaveLoadMechanism extends iron.Trait {
    //Get keyboard's input.
    var kb = Input.getKeyboard();

	public function new() {
		super();

        notifyOnUpdate(function() {
            //Call save() when 'f' key is pressed.
            if(kb.started("f")){
                save();
            }
        });
	}

    public function save(){
        #if kha_krom
        var saveData = { text: "Hello World!" };
        var saveDataJSON = haxe.Json.stringify(saveData);

        // Adds '/../../../' to path to move out of 3 directories.
        // And then go to bundled/save_game.json.
        var path = Krom.getFilesLocation() + "/../../../" + "/Bundled/save_game.json";

        var bytes = haxe.io.Bytes.ofString(saveDataJSON);
        Krom.fileSaveBytes(path, bytes.getData());
        trace("Saved!");
        #end
    }
}
```

![savejsonbundled](/../../docassets/save_load_4.png ':size=700')

!> Don't forget to create Bundled folder in your root directory!

We will now add reading functionality, we will read `save_game.json` from `Bundled` folder and print `text` value in debug console. To enable debug console for debugging(ofc!), head over to `Render - Armory Project - Flags` and select `Debug Console`.

![debugconsole](/../../docassets/save_load_5.png ':size=700')

Now let get to code:
```haxe
// In SaveLoadMechanism.hx

package arm;

import iron.system.Input;
import iron.data.Data;

class SaveLoadMechanism extends iron.Trait {

    var kb = Input.getKeyboard();

    var saveFile = "save_game.json";

	public function new() {
		super();

		notifyOnUpdate(function() {
            if(kb.started("f")){
                save();
            }else if(kb.started("g")){
                load();
            }
		});
	}

    public function save() { ~ }


    public function load(){
        //Get Blob, it director is defaulty set to bundled.
        Data.getBlob(saveFile, function(bytes:kha.Blob) {
            //Converts bytes to string.
            var jsonString = bytes.toString();

            //Parse value from stringy json.
            var json = haxe.Json.parse(jsonString);
            trace(json.text);
        });
    }
}
```

Hit `Play`, try pressing `f` and then `g`, `Hello World!` should appear in debug console, if it do than that means saving and loading work perfectly!

![printdebugconsole](/../../docassets/save_load_6.png ':size=700')

And that is for today! In next part we will make it save cube's Location and Rotation.