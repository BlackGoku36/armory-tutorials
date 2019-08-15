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

WIP
{: .label .label-red}