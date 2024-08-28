# Dead Cells Mods documentation 
### 中英翻譯版本

由 lamaJL1126 使用 DeepL 和 ChatGPT 共同協助 翻譯 官方模組文檔(ModsDoc.pdf)，並個人再加以修飾、校對，以及添加更多的具體教學。新添的教學多加註於中文內容中，或修改部分翻譯，使其更易於使用、理解。

內容為中英對照，以方便操作時可以與官方文檔交互確認。


## Document purpose
This document explains in detail how the game data workflow works, how to manipulate its data, create scripts for it and upload mods to the Steam workshop. It’s targeted to people who want to create mods and it explains everything from scratch, step by step. We’ll make the difference between data modifications and scripting. If you already played with the pak and cdb there is probably a lot in here that you already know.

## 文件目的
本文件將詳細解釋遊戲資料工作流程的運作方式、如何操作其資料、為其建立腳本、以及如何上傳 mod 到 Steam workshop。文件目標對象是想要製作 mod 的玩家，它將會從頭開始解釋一切。我們會將內容區分成資料修改和腳本修改。如果你已經接觸過 pak 檔案 和 cdb 命令列，此文件可能會有很多你已知的內容。


## General presentation
There is a major difference between the flow for altering data and creating some scripts. The process of creating scripts for a mod is more straightforward but requires more technical skills.

## 概要
在修改數據和編寫腳本的流程之間存在一個主要區別。為模組編寫腳本的過程更簡單直接，但難度較高。


- ### Brief presentation of tech and production flows to modify data
  The game uses a big file to store its data: the PAK. If you browse your game files, you can see it as “res.pak” at the root of the installation directory of Dead Cells. It embeds all files used by the game.

  To modify any data in the game you need to be able to open and extract every file from this pak. It’s one of the purposes of PAKTool.

  With mods we introduced the possibility, for what we call a “mod pak”, to override data present in the original pak. This mod pak will be loaded during runtime and each file it contains will override the original ones. There are few exceptions with the CDB file and room edition with Tiled, for more detailed information report to the CDBTool & Rooms.

  So, to create a mod, you have to extract files from the res.pak, modify the extracted files and then rebuild a mod pak. This mod pak will be created with the PAKTool and contain only files you modified from the original res.pak (to be as light as possible).

  To run the tools, you need to install (or already have installed the .NET Framework version 4.5.2 or above. You can find it on Microsoft site here : https://www.microsoft.com/fr-fr/download/details.aspx?id=42642

  ### 修改數據的方式和流程概要
  遊戲使用一個 pak 檔案來儲存所有資料。可以在 Dead Cells 安裝目錄的根目錄中看到「res.pak」，它內含遊戲所使用的所有資料。

  若要修改遊戲中的任何一筆資料，你必須有權限開啟並擷取此 pak 中的每個檔案。這是「PAKTool」的用途之一。

  此遊戲的 mod 運作方式較為另類，稱之為「mod pak」的方式，是以覆蓋原始 pak 中的資料運行。mod 套件在執行時載入，其中包含的每個檔案都會取代原始檔案，反之若有未包含的檔案則沿用原始檔案。「CDB 檔案」和「使用 Tiled 進行關卡修改」會有少數例外，詳細資訊，參閱 CDBTool & Rooms。

  因此、若要製作一個 mod，你必須從「res.pak」中獲得原始檔案，修改獲得的原始檔案，然後再重建一個 mod 的 pak 檔案。此 mod pak 將使用「PAKTool」建立，並且僅包含 mod 中從原始「res.pak」修改的檔案 (以盡可能減輕檔案大小)。
  
  要運行這些工具(「PAKTool」~)，你需要安裝「.NET Framework」版本 4.5.2 或更高版本（如果你的電腦尚未安裝）。你可以在 Microsoft 的網站上找到它：https://www.microsoft.com/fr-fr/download/details.aspx?id=42642

  
- ### Brief presentation of scripting flow
  The game now supports scripting for creating level structures, defining level info and mob roster in a level. The flow is very simple, when a mod is activated, it checks for the presence of a directory ./Script/Struct/ in the mod’s directory and tries to load scripts present in it. It’s compatible with some res.pak modifications (you can change the res.pak and have scripts in the same mod).
  ### 使用腳本定義的方式和流程概要
  遊戲支援使用腳本來製作關卡結構、定義關卡信息和關卡中生成的敵人。流程概括為，當一個 mod 被加載時，遊戲會先掃描 mod 目錄中是否存在 ./Script/Struct/ 目錄，並嘗試加載其中的腳本。加載的腳本可以與某些「res.pak」修改兼容（你可以在一個 mod 中同時包含修改的「res.pak」和 腳本）。


