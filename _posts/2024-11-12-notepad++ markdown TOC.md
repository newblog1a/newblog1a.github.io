---
published: true
---
open explorer administrator
  Open This PC by pressing Windows + E key, and then navigate to C: Windows. Step 2: Find and right-click on explorer.exe

add
```xml
			  <association id=  "markdown.xml"     userDefinedLangName="Markdown (preinstalled)" />
			  <association id=  "markdown.xml"     ext=".md"                               />
			  <association id=  "markdown.xml"     ext=".markdown"                         />
```
in overrideMap.xml in %ProgramFiles%\functionList
  https://community.notepad-plus-plus.org/topic/20478/how-to-add-an-new-entry-in-the-functionlist/2

add markdown.xml in %ProgramFiles%\functionList:
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!-- ==========================================================================\
|
|   To learn how to make your own language parser, please check the following
|   link:
|       https://npp-user-manual.org/docs/function-list/
|
\=========================================================================== -->
<NotepadPlus>
	<functionList>
		<!-- ================================================ [ markdown ] -->

		<parser
			displayName="markdown.xml"
			id         ="markdown.xml"
			commentExpr=""
		>
			<function
				mainExpr="(?x)((?:^|\n)[#]+\s+(.*?)(\r*|\n*)$)"
			>
				<functionName>
					<nameExpr expr="(?:[#]+\s+)(.*)" />
				</functionName>
			</function>
		</parser>
	</functionList>
</NotepadPlus>
```
ref
The "functionList.xml" files from "%ProgramFiles%" and "%APPDATA%\notepad++"
add functionList.xml
  https://community.notepad-plus-plus.org/topic/18808/plugin-request-markdown-navigation-panel/2
