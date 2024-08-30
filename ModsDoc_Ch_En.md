# Dead Cells Mods documentation 
### 中英翻譯版本
由 lamaJL1126 使用 DeepL 和 ChatGPT 共同協助 翻譯 官方模組文檔(ModsDoc.pdf)，並個人再加以修飾、校對，以及添加更多的具體教學。新添的教學多加註於中文內容中，或修改部分翻譯，使其更易於使用、理解。

內容為中英對照，以方便操作時可以與官方文檔交互確認。


# Document purpose
This document explains in detail how the game data workflow works, how to manipulate its data, create scripts for it and upload mods to the Steam workshop. It’s targeted to people who want to create mods and it explains everything from scratch, step by step. We’ll make the difference between data modifications and scripting. If you already played with the pak and cdb there is probably a lot in here that you already know.

## 文件目的
本文件將詳細解釋遊戲資料工作流程的運作方式、如何操作其資料、為其建立腳本、以及如何上傳 mod 到 Steam workshop。文件目標對象是想要製作 mod 的玩家，它將會從頭開始解釋一切。我們會將內容區分成資料修改和腳本修改。如果你已經接觸過 pak 檔案 和 cdb 命令列，此文件可能會有很多你已知的內容。


# General presentation
There is a major difference between the flow for altering data and creating some scripts. The process of creating scripts for a mod is more straightforward but requires more technical skills.

## 概要
在修改數據和編寫腳本的流程之間存在一個主要區別。為模組編寫腳本的過程更簡單直接，但難度較高。


- ## Brief presentation of tech and production flows to modify data
  The game uses a big file to store its data: the PAK. If you browse your game files, you can see it as “res.pak” at the root of the installation directory of Dead Cells. It embeds all files used by the game.

  To modify any data in the game you need to be able to open and extract every file from this pak. It’s one of the purposes of PAKTool.

  With mods we introduced the possibility, for what we call a “mod pak”, to override data present in the original pak. This mod pak will be loaded during runtime and each file it contains will override the original ones. There are few exceptions with the CDB file and room edition with Tiled, for more detailed information report to the CDBTool & Rooms.

  So, to create a mod, you have to extract files from the res.pak, modify the extracted files and then rebuild a mod pak. This mod pak will be created with the PAKTool and contain only files you modified from the original res.pak (to be as light as possible).

  To run the tools, you need to install (or already have installed the .NET Framework version 4.5.2 or above. You can find it on Microsoft site here : https://www.microsoft.com/fr-fr/download/details.aspx?id=42642

  ### 修改數據的方式和流程概要
  遊戲使用一個 pak 檔案來儲存所有資料。可以在 Dead Cells 安裝目錄的根目錄中看到「res.pak」，它內含遊戲所使用的所有資料。

  若要修改遊戲中的任何一筆資料，你必須有權限開啟並擷取此 pak 中的每個檔案。這是「PAKTool」的用途之一。

  此遊戲的 mod 運作方式較為另類，稱之為「mod pak」的方式，是以覆蓋原始 pak 中的資料運行。mod 套件在執行時載入，其中包含的每個檔案都會取代原始檔案，反之若有未包含的檔案則沿用原始檔案。「CDB 檔案」和「使用 Tiled 進行關卡修改」會有少數例外，詳細資訊，參閱「CDBTool & Rooms」。

  因此若要製作一個 mod，你必須從「res.pak」中獲得原始檔，修改獲得的原始檔，然後再重建一個 mod 的 pak 檔案。此 mod pak 將使用「PAKTool」建立，並且僅包含與原始「res.pak」不同的檔案 (以盡可能減輕檔案大小)。
  
  要運行這些工具(「PAKTool」~)，你需要安裝「.NET Framework」版本 4.5.2 或更高版本（如果你的電腦尚未安裝）。你可以在 Microsoft 的網站上找到它：https://www.microsoft.com/fr-fr/download/details.aspx?id=42642

  
- ## Brief presentation of scripting flow
  The game now supports scripting for creating level structures, defining level info and mob roster in a level. The flow is very simple, when a mod is activated, it checks for the presence of a directory ./Script/Struct/ in the mod’s directory and tries to load scripts present in it. It’s compatible with some res.pak modifications (you can change the res.pak and have scripts in the same mod).

  ### 腳本簡介和運作流程概要
  遊戲支援使用腳本來製作關卡結構、定義關卡信息和關卡中生成的敵人。流程概括為、當一個 mod 被加載時，遊戲會先掃描 mod 中是否存在 ./Script/Struct/ 目錄，並嘗試加載其中的腳本。加載的腳本可以與某些「res.pak」修改兼容（你可以在一個 mod 中同時包含修改的「res.pak」和腳本以修改遊戲）。


- ## The types of files
  ### 檔案類型

  
  - ### res.pak
    As stated before, the res.pak is a container for all game files. It’s in the installation root directory. It can be expanded using the PAKTool.

    「res.pak 」它位於安裝根目錄中，是一個包含所有遊戲資料的檔案。可以使用「PAKTool」進行解壓。


  - ### data.cdb
    Data.cdb is a file containing a lot of different things concerning both gameplay and cosmetics. It’s a big JSON file and can be edited with CastleDB. It contains rooms layouts, weapon parameters, fogs color etc...

    「data.cdb」是一個大型的 JSON 檔案，包含了遊戲玩法和外觀相關的內容，包括房間佈局、武器參數、霧的顏色等。可以使用「CastleDB」進行編輯。


  - ### .atlas & .png
    In Dead Cells, all textures are PNGs and most of them are atlases. An atlas is multiple PNGs merged in one big PNG. In Dead Cells there are (few) standalone PNGs and .png/.atlas pairs. .atlas files contain information on where and how big a given image is stored in the .png. The AtlasTool is here to expand atlases in several “original files” to allow you to modify them and then rebuild the atlas from expanded files.

    在遊戲中所有的材質都是 PNG 檔案(.png)，其中大多是圖集(將多個 PNG 合併為一個大的 PNG 的技術)，但同時也存在部分獨立的 PNG 檔案，以及成對的 PNG 和「.atlas」檔案(「.atlas」定義了相對圖集中的每個圖像儲存位置及大小等信息)。可以使用「AtlasTool」將圖集展開為多個原始檔以便修改，並將展開後的檔案重新組合成圖集。


  - ### .hx
    They are script code files. They are loaded and interpreted by the game during different steps like: creating the structure of a level, setting level parameters, generating mob roster, spawning mobs etc.

    腳本程式碼檔案，在各個的步驟中，如創建關卡結構、設置關卡參數、生成敵人列表、生成敵人等，這些腳本會被遊戲加載並執行。


  - ### .tmx
    Those files are located in tiled/tmx in the expanded pak folder. They contain the data of the rooms in the Tiled tool format. Be careful, the tmx stored in the pak and used by the game is actually a binary file, that can’t be opened with Tiled. In order to edit them, you will have to export them with TmxTool.

    這類檔案位於展開的 pak 資料夾中的 tiled/tmx 目錄下。檔案以「Tiled」工具的格式儲存其中包含房間的數據(「Tiled」是一個用於修改和創建房間佈局設計的軟體)。要注意的是 pak 中存儲並被遊戲使用的 tmx 檔案實際上是二進制檔案，無法用 Tiled 打開並編輯。要編輯這些檔案，需要先使用「TmxTool」將二進制的 tmx 檔案轉換。
    

- ## How mods will behave in the game
  The game supports multiple mods activated at the same time. In some cases, it will consider that two mods are incompatible with each other and prevent from activating the second one. In that case a message will warn the user on why the mod cannot be activated.
  
  Two mods will be considered incompatible if one (or more) of this proposition is true:
  - Both mods contain scripts
  - Both mods modify the same file (except for the CDB)
  - Both mods modify the same line of the CDB
    
  A mod can only be activated when creating a new save or restarting a run from prison start.

  For now, with each update (even minor ones) mods will be deactivated and current saves using mods will be reset (only the run, not meta items). This is to prevent the game from crashing and/or corrupt the saves.

  ### 模組在遊戲中的加載和行為
  遊戲支持同時加載複數模組，但在特殊情況中，遊戲會認為兩個(或多個)模組互不兼容，並阻止加載其中一個模組，系統將會輸出訊息提示模組為何無法加載。

  若以下條件中有一個(或多個)成立，兩個模組就會被視為不兼容：
  - 兩個模組都包含腳本。
  - 兩個模組修改了相同的檔案(CDB 檔案除外)。
  - 兩個模組修改了 CDB 中的同一行程式。

  模組只能在創建新存檔或重新開始遊戲時加載。

  每次更新（包含小更新），為了防止遊戲崩潰或損壞存檔，模組會被暫時停用，並且使用模組的當前存檔將會被重置進度（僅重置遊戲進度，不包含永久升級內容）。


- ## What you can do with mods (current state)
  This list is non-exhaustive
  - Change some atlases (or part of atlases) (see list of unalterable atlases)
  - Add floor junks
  - Modify parameters in the CDB (except for table “Truelle”)
  - Create script to define a customized level structure, world graph, mob roster

  ### 模組當前可進行的操作
  包括下列但不限於下列
  - 更改部分圖集（參見不可更改的圖集列表）
  - 添加地板上的雜物
  - 修改 CDB 中的參數（除了「Truelle」表）
  - 創建腳本來定義自訂的關卡結構、世界圖、敵人列表


- ## What you cannot do with mods (current state)
  All this may change with future releases and the list is non-exhaustive.

  For now you cannot modify some atlases:
  - atlas/ui.atlas
  - atlas/fxCommon.atlas
  - atlas/ui.atlas
  - atlas/fxCommon.atlas
  - atlas/fxEnemy.atlas
  - atlas/fxWeapon.atlas
  - atlas/fxDisplace.atlas
  - atlas/gameElements.atlas
  - atlas/lore.atlas

  For caching reasons, we cannot reload those during runtime and modifying them will result in a no-op

  You cannot modify the table “Truelle” in data.cdb. It will be a no-op too.

  Some parameters in the CDB may have no effect when changed, but there is no exhaustive list at this point.

  Removing lines of separators in the CDB may cause the game to crash.

  Removing entries from atlases may cause the game to crash or graphic bugs (depending on which atlas is concerned).

  Generally removing files or part of files will cause the game to crash at one point or another.

  Language should be added or altered via the existing process: https://steamcommunity.com/games/588650/announcements/detail/1253537422578152950

  ### 模組當前不可進行的操作
  包括下列但不限於下列，且下列內容都可能因未來版本推出而改變。

  目前無法修改以下圖集：
  - atlas/ui.atlas
  - atlas/fxCommon.atlas
  - atlas/ui.atlas
  - atlas/fxCommon.atlas
  - atlas/fxEnemy.atlas
  - atlas/fxWeapon.atlas
  - atlas/fxDisplace.atlas
  - atlas/gameElements.atlas
  - atlas/lore.atlas

  基於緩存原因，遊戲無法在運行後加載模組時重新加載這些圖集，修改它們將不會產生任何效果(在模組中，由於遊戲啟動後才會加載模組，因此存在於模組中的此類圖集無法修改遊戲中已存在於緩存的原有的圖集)。

  無法修改「data.cdb」中的「Truelle」表，修改將會是無效的。

  修改 CDB 中的某些參數可能不會產生效果，但目前沒有詳細的列表。

  移除 CDB 中的分隔線可能會導致遊戲崩潰。

  從圖集中移除項目可能會導致遊戲崩潰或出現圖形錯誤（依具體圖集而定）。

  正常情況下，移除檔案或移除檔案的一部分最終會導致遊戲崩潰。

  添加或修改語言應通過現有的流程：https://steamcommunity.com/games/588650/announcements/detail/1253537422578152950。


# Tools list
Except for castleDB, the tools presented are command line tools.

They accept - and / at the start of arguments. Argument names are NOT case sensitive.

Order of arguments is not important (as long as you respect the order -argument <parameter> ):

Example:
```
-EXPAND -OUTDIR "C:\Tests\ExpandedAtlas\kingsGuardian\" -ATLAS "C:\Tests\Expanded\atlas\kingsGuardian.atlas"
```
is equivalent to
```
-ATLAS "C:\Tests\Expanded\atlas\kingsGuardian.atlas" -OUTDIR "C:\Tests\ExpandedAtlas\kingsGuardian\" -EXPAND
```

## 工具列表









