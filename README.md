# Pirates D3D9 Hook  

This patch for *Sid Meier's Pirates!* (2004) modifies the game executable to allow loading a local `d3d9.dll`, enabling compatibility with alternate Direct3D 9 DLLs such as the RTX Remix bridge.  

## **How It Works**  
By default, the exe runs the game in a separate process, this process loads `d3d9.dll` from the system directory. This patch modifies the executable to search for `.\d3d9.dll` instead, allowing the use of custom Direct3D 9 implementations.  

## **Disclaimer **
To prevent any copyright infringement issues, I do not provide the unencrypted or patched executable, you have to get it on your own means.
This has been tested on the steam release of the game. I am not sure that the Xdelta patch works on other releases, but the manual patch should work.

## **Requirements**  
- A **DRM-free + unencrypted** copy of the game executable  
  - Obtainable via [Adam Milazzo's Challenge Generator](http://www.adammil.net/blog/v121_The_Sid_Meier_s_Pirates_Challenge_Pack.html)  
  - Do not apply any mods or options—only extract the unencrypted EXE  

## **Manual Patch Instructions**  
If you prefer to modify the executable manually, follow these steps:  

1. Open the game executable in a **hex/PE editor** (HxD, Ghidra, etc.).  
2. Locate all occurrences of `d3d9.dll`. These may appear in different capitalization styles (`D3D9.DLL`, `d3d9.dll`, `D3D9.dll`, etc.).  
3. Modify each instance to `.\d3d9.dll` (while preferably keeping the same capitalization).  
   - **Ensure there is at least one null byte (`00`) before and after each modified string.**  
   - Depending on the number of null bytes beforehand, some entries only require prefixing `.\`, while others require rewriting the string.  
4. Modify the following memory addresses with the specified values:  

   | Address  | Original | Modified | Comment |
   |----------|---------|---------|---------|
   | 002DF360 | 44 65 62 75 67 53 65 74 4D 75 74 65 00 00 00 00 | 44 65 62 75 67 53 65 74 4D 75 74 65 00 00 2E 5C | This "line" is before the occurence of d3d9.dll, only writing `.\` before it |
   | 00312C90 | 8C 22 5F 00 44 33 44 39 2E 44 4C 4C 00 00 00 00 | 8C 22 5F 00 2E 5C 44 33 44 39 2E 44 4C 4C 00 00 | |
   | 00312D00 | 63 74 33 44 43 72 65 61 74 65 39 00 44 33 44 39 | 63 74 33 44 43 72 65 61 74 65 39 00 2E 5C 44 33 | Here, the string is split in the two "lines" |
   | 00312D10 | 2E 64 6C 6C 00 00 00 00 25 35 64 4B 62 20 28 25 | 44 39 2E 64 6C 6C 00 00 25 35 64 4B 62 20 28 25 | |

## **Xdelta Patch (Recommended)**  
For an easier solution, use the provided **Xdelta patch**.
The patch can be found in the repository as `pirates-d3d9-hook.xdelta`.

You can find instructions about how to use Xdelta as well as download links on [PCGamingWiki](https://www.pcgamingwiki.com/wiki/Xdelta)

## **Why Use This?**  
- Enables **RTX Remix** and other custom Direct3D 9 implementations  
- Compatible with existing mods that hook into Direct3D  
- Does not alter gameplay or introduce instability  

## **License**  
This project is licensed under the **MIT License**.  
