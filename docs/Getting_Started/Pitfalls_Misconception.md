## Misconceptions {docsify-ignore-all}

<details>
	<summary>I choosed `EEVEE`/`Cycles` renderer but still get this error</summary>

<p style="margin-left: 35px">It doesn't matter which renderer you select, Armory has it own renderer, and `EEVEE`/`Cycles`/`Workbench` aren't used by Armory at all.</p>
</details>

---

## Common Pitfalls

<details>
	<summary> I am using Blender 2.7...</summary>

<p style="margin-left: 35px">Let me stop you right there, Armory now uses Blender 2.8 and support for Blender 2.7X is dropped, If you want to use Blender 2.7x, you will have to use Armory version < 0.6, you can find it [here](https://github.com/armory3d/armory/releases). So, move on like I did.</p>
</details>

---

<details>
	<summary>I made changes to my game. Why don’t they show up when I start the game?</summary>

<p style="margin-left: 35px">Armory caches builds of the game. Sometimes you need to clean this cache, click `Clean` button next to `Play` button under `Render Tab` in `Armory Player`, or if you don't want to clean build everytime, you can entirely disable it by `Render Tab - Armory Project - Flags` and unselect `Cache Build`.</p>
</details>

---

<details>
	<summary>My mesh look all weird, how do I fix this?</summary>

<p style="margin-left: 35px">That issue is because of triangulation bug, try clean building.</p>
</details>

---

<details>
	<summary>Error says that it can't find some file</summary>

<p style="margin-left: 35px">Make sure your blend file isn't directly exposed to any drive and also check if your folder's name doesn't have any illegal characters.</p>
</details>

---
