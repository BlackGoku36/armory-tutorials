# Save/Load Mechanism

WIP
{: .label .label-red}

Let create save/load mechanism for our game! We should be able to save and load object location/rotation and some other thing at the end of tutorial.

### Setup
Let first open and save default cube blend file and in `Render - Armory Player` hit `Play`, a [Krom](https://github.com/Kode/Krom) window should pop-up showing our default cube. If it do then, celebrate! you have armory working!

![default_cube](Assets/save_load_1.png)

Now, in `Scene - Armory Scene Traits` create a haxe script with whatever name you want(`SaveLoadMechanism` in my case) and then hit `Edit Script`, now it should open [Kode Studio](https://github.com/Kode/KodeStudio) (if you have it install) or your system default IDE.

Note: Make sure to make your script's first letter to be capital
{: .label .label-yellow}


![VSCode](Assets/save_load_2.png)

Now, for learning sake, we will be using [json](https://en.wikipedia.org/wiki/JSON) to store and read information from, later you can do stuff to make it unreadable to us humans to prevent cheating. [Haxe](https://haxe.org/) provide really good support for json and xml parsing and writing, making it easier for us to use.

```
// In SaveLoadMechanism.hx

package arm;

class SaveLoadMechanism extends iron.Trait {
    public function new() {
        super();
        notifyOnInit(function() {

            #if kha_krom // 1
            var saveData = { text: "Hello World!" }; // 2
            var saveDataJSON = haxe.Json.stringify(saveData); // 3

            // will be saved at root_folder/build_file/debug/krom/my_file.json
            var path = Krom.getFilesLocation() + "/save_game.json"; // 4

            // Write file
            var bytes = haxe.io.Bytes.ofString(saveDataJSON); // 5
            Krom.fileSaveBytes(path, bytes.getData()); // 6
            #end
        });

        // notifyOnUpdate(function() {
        // });
    }
}
```
Let go and understand the code line-by-line:
1. This is `guard`, it will tell [Kha](https://github.com/Kode/Kha) to use code(in-between guard) when guard's condition is met, here it will tell Kha to save our file only if the target the game is running on is Krom. We are using it here so that the game will not cause any errors because we are using target specific function in our code `// 6`
2. Here variable `saveData` have little json structure defined, it only have `text: Hello World!` for now, we will add more as we go.
3. haxe.Json.stringify will convert our json structure to string json for saving to file.
4. Krom.getFilesLocation() will get path of build folder and add `"/save_game.json"` to it for complete path.
5. This will take our `saveDataJSON` string and than it will write it to bytes to save.
6. Krom.fileSaveBytes(path, bytes.getData()); will save bytes to path specified.

Now, hit `Play` and when you go over to `root_folder/build_file/debug/krom/` you should find `save_game.json` and on opening it should read `{"text":"Hello World!"}`, if you do, then Congratulation! You did it!

![savejson](Assets/save_load_3.png)

Now, let save `save_game.json` to proper place and add some keyboard input code to handle saving manualy instead of saving when the game initiate.

```
package arm;

import iron.system.Input;// 1

class SaveLoadMechanism extends iron.Trait {
	public function new() {
		super();

		var kb = Input.getKeyboard();// 1

        notifyOnUpdate(function() { // 2
            #if kha_krom
            var saveData = { text: "Hello World!" };
            var saveDataJSON = haxe.Json.stringify(saveData);

            // will be saved at file_write/build_file/debug/krom/my_file.json
            var path = Krom.getFilesLocation() + "/../../../" + "/Bundled/save_game.json"; // 3

            // Write file
            var bytes = haxe.io.Bytes.ofString(saveDataJSON);
            // 4
            if (kb.started("f")){
                Krom.fileSaveBytes(path, bytes.getData());
                trace("Saved!");
            }
            #end
        });
	}
}
```
1. We import and initialize keyboard input variable.
2. We change `notifyOnInit` to `notifyOnUpdate`.
3. Here, we get out of build files and put get bundled path, this will help us in reading the `save_game.json` later (`/..` means out of a directory).
4. This check if keyboard key `f` is started, if so then it will save the file.
Now, if you play and press `f` then it should save `save_game.json` to Bundled.

![savejsonbundled](Assets/save_load_4.png)

Don't forget to create Bundled folder in your root directory!
{: .label .label-yellow}

We will now add reading functionality, we will read `save_game.json` from `Bundled` folder and print `text` value in debug console. To enable debug console for debugging(ofc!), head over to `Render - Armory Project - Flags` and select `Debug Console`.

![debugconsole](Assets/save_load_5.png)

Now let get to code:
```
package arm;

import iron.system.Input;
import iron.data.Data;//<===
    
                          ~
		var saveFile = "save_game.json";//<===

		notifyOnUpdate(function() {
                          ~
			var path = Krom.getFilesLocation() + "/../../../" + "/Bundled/$saveFile";//<===
                          ~
			if(kb.started("g")){
				Data.getBlob(saveFile, function(bytes:kha.Blob) {
					var jsonString = bytes.toString();// 1

					// Get json from string
					var json = haxe.Json.parse(jsonString);// 2
					trace(json.text);// 3
				});
			}
		});
	}
}
```
1. We convert loaded bytes to string and assign it to `jsonString`
2. Parse json from `jsonString`
3. We finally print/trace `text` from json

Hit `Play`, try pressing `f` and then `g`, `Hello World!` should appear in debug console, if it do than that means saving and loading work perfectly!

![printdebugconsole](Assets/save_load_6.png)

WIP
{: .label .label-red}