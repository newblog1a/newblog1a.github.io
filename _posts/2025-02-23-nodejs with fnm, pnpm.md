choco install -y fnm
fnm install --lts

add 
  eval "$(fnm env --use-on-cd --shell bash)"
in ~/.bashrc
reopen cmder

ref
cmder in C:\tools\Cmder
  .bashrc
  https://github.com/cmderdev/cmder/issues/565

choco install -y pnpm

Q: package show error1
A: choco install -y uv
new error:
reason: When Python is installed by uv, it will not be available globally
A: choco install python visualstudio2022-workload-vctools -y
  https://github.com/ts-safeql/safeql/issues/255
new error:
Installing visualstudio2022-workload-vctools results in an error.
A: but npm install ok

Q: package show error: 
```cmd
> ./smoke-test.js

'.' is not recognized as an internal or external command,
operable program or batch file.
```
A: change 
```json
  "scripts": {
    "bloggerconvert": "./index.js",
```
to 
```json
  "scripts": {
    "bloggerconvert": "node ./index.js",
```

error:
```
john@SKY-20241029KDE ~/a/blogger-archive-converter-master
λ pnpm approve-builds
√ Choose which packages to build (Press <space> to select, <a> to toggle all, <i> to invert selection) · node-expat √ The next packages will now be built: node-expat.
Do you approve? (y/N) · true
node_modules/.pnpm/node-expat@2.4.1/node_modules/node-expat: Running install script, failed in 26ms
.../node_modules/node-expat install$ node-gyp rebuild
│ 'node-gyp' is not recognized as an internal or external command,
│ operable program or batch file.
└─ Failed in 26ms at C:\Users\john\a\blogger-archive-converter-master\node_modules\.pnpm\node-expat@2.4.1\node_modules\node-expat
 ELIFECYCLE  Command failed with exit code 1.
john@SKY-20241029KDE ~/a/blogger-archive-converter-master
λ pnpm install node-gyp
 WARN  5 deprecated subdependencies found: hoek@4.3.1, hoek@5.0.4, hoek@6.1.3, joi@13.7.0, topo@3.0.3
Packages: +56
++++++++++++++++++++++++++++++++++++++++++++++++++++++++
Progress: resolved 159, reused 103, downloaded 56, added 56, done

dependencies:
+ node-gyp 11.1.0

Done in 14.3s using pnpm v10.4.1
john@SKY-20241029KDE ~/a/blogger-archive-converter-master
λ pnpm approve-builds
There are no packages awaiting approval
john@SKY-20241029KDE ~/a/blogger-archive-converter-master
λ pnpm run test

> blogger-archive-converter@0.1.0 pretest C:\Users\john\a\blogger-archive-converter-master
> npm run bloggerconvert ./test-data/test-blog-with-simple-default-theme.xml


> blogger-archive-converter@0.1.0 bloggerconvert
> node ./index.js ./test-data/test-blog-with-simple-default-theme.xml

C:\Users\john\a\blogger-archive-converter-master\node_modules\.pnpm\bindings@1.5.0\node_modules\bindings\bindings.js

:121
        throw e;
        ^

Error: \\?\C:\Users\john\a\blogger-archive-converter-master\node_modules\.pnpm\node-expat@2.4.1\node_modules\node-ex

pat\build\Release\node_expat.node is not a valid Win32 application.
\\?\C:\Users\john\a\blogger-archive-converter-master\node_modules\.pnpm\node-expat@2.4.1\node_modules\node-expat\bui

ld\Release\node_expat.node
    at Object..node (node:internal/modules/cjs/loader:1732:18)
    at Module.load (node:internal/modules/cjs/loader:1289:32)
    at Function._load (node:internal/modules/cjs/loader:1108:12)
    at TracingChannel.traceSync (node:diagnostics_channel:322:14)
    at wrapModuleLoad (node:internal/modules/cjs/loader:220:24)
    at Module.require (node:internal/modules/cjs/loader:1311:12)
    at require (node:internal/modules/helpers:136:16)
    at bindings (C:\Users\john\a\blogger-archive-converter-master\node_modules\.pnpm\bindings@1.5.0\node_modules\bin

dings\bindings.js:112:48)
    at Object.<anonymous> (C:\Users\john\a\blogger-archive-converter-master\node_modules\.pnpm\node-expat@2.4.1\node

_modules\node-expat\lib\node-expat.js:4:34)
    at Module._compile (node:internal/modules/cjs/loader:1554:14) {
  code: 'ERR_DLOPEN_FAILED'
}

Node.js v22.14.0
 ELIFECYCLE  Command failed with exit code 1.
john@SKY-20241029KDE ~/a/blogger-archive-converter-master
λ pnpm install
Lockfile is up to date, resolution step is skipped
Already up to date
Done in 832ms using pnpm v10.4.1
john@SKY-20241029KDE ~/a/blogger-archive-converter-master
λ pnpm approve-builds
There are no packages awaiting approval
john@SKY-20241029KDE ~/a/blogger-archive-converter-master
λ
john@SKY-20241029KDE ~/a/blogger-archive-converter-master
λ pnpm rebuild node-expat
node_modules/.pnpm/node-expat@2.4.1/node_modules/node-expat: Running install script, done in 11s
```

resean:
The error message indicates that `node-expat.node` is not a valid Win32 application, which likely means:

1.  The module `node-expat` was not compiled for your Node.js version.
2.  There is a mismatch between your Node.js architecture and the prebuilt binary.
3.  The package needs to be rebuilt.

### Possible Fixes

#### 1\. Rebuild the Package

Try rebuilding `node-expat` to ensure it is compiled correctly for your environment:

`pnpm rebuild node-expat`


---
pnpm install:
```cmd
╭ Warning ───────────────────────────────────────────────────────────────────────────────────╮
│                                                                                            │
│   Ignored build scripts: node-expat.                                                       │
│   Run "pnpm approve-builds" to pick which dependencies should be allowed to run scripts.   │
│                                                                                            │
╰────────────────────────────────────────────────────────────────────────────────────────────╯
```

npm install:
```cmd
npm error command failed
npm error command C:\Windows\system32\cmd.exe /d /s /c node-gyp rebuild
npm error gyp info it worked if it ends with ok
npm error gyp info using node-gyp@11.0.0
npm error gyp info using node@22.14.0 | win32 | x64
npm error gyp ERR! find Python
npm error gyp ERR! find Python Python is not set from command line or npm configuration
npm error gyp ERR! find Python Python is not set from environment variable PYTHON
npm error gyp ERR! find Python checking if the py launcher can be used to find Python 3
```

npm Windows-Build-Tools:
```
This package has been deprecated

Author message:

Node.js now includes build tools for Windows. You probably no longer need this tool. See https://github.com/felixrieseberg/windows-build-tools for details.
```

```
在 Windows 10 环境安装 node-v16.13.1-x64.msi 的时候。
会自动安装 Python 依赖。

在 NodeJS 的安装过程中，勾选Authmatically install the necessary tools
```
  https://blog.csdn.net/wuyujin1997/article/details/122288705
  
reason:
用fnm安裝，不會安裝python

ref
Unable to install using npm
  https://github.com/ts-safeql/safeql/issues/255

---
Installing visualstudio2022-workload-vctools results in an error.:
```
visualstudio2022-workload-vctools v1.0.0 [Approved] - Possibly broken
visualstudio2022-workload-vctools package files install completed. Performing other installation steps.
Installing visualstudio2022-workload-vctools...

[6f88:0001][2025-02-23T13:17:59] Saving the current locale (zh-TW) to user.json.
[6f88:0001][2025-02-23T13:17:59] Setting the telemetry services
[6f88:0005][2025-02-23T13:18:00] Creating a new telemetry service.
[6f88:0001][2025-02-23T13:18:00] Visual Studio Installer Version: 3.13.2069
[6f88:0001][2025-02-23T13:18:00] Raw Command line: "C:\Program Files (x86)\Microsoft Visual Studio\Installer\setup.e
xe" modify --installPath "C:\Program Files (x86)\Microsoft Visual Studio\2022\BuildTools" --includeRecommended --nor
estart --quiet --add Microsoft.VisualStudio.Workload.VCTools
[6f88:0001][2025-02-23T13:18:00] Parsed command line options: modify --add Microsoft.VisualStudio.Workload.VCTools -
-includeRecommended --installPath "C:\Program Files (x86)\Microsoft Visual Studio\2022\BuildTools" --norestart --qui
et
[6f88:0001][2025-02-23T13:18:00] Using registry value of (1) to theme installer.
[6f88:0001][2025-02-23T13:18:00] Getting singleton lock. Mutex name: DevdivInstallerUI
[6f88:0001][2025-02-23T13:18:00] Getting singleton lock succeed.
[6f88:0005][2025-02-23T13:18:00] Telemetry session ID: ad600ad1-4c4a-41c5-b51f-db3e7f515fc1
[6f88:0001][2025-02-23T13:18:00] Creating new ExperimentationService
[6f88:0001][2025-02-23T13:18:00] Telemetry property VS.ABExp.Flights : lazytoolboxinit;fwlargebuffer;refactoring;spm
oretempsbtn1;asloff;keybindgoldbarext;asynccsproj;vsfricheditor;completionapi;4f604693:30775293;7a6g6897:31242874
[6f88:0005][2025-02-23T13:18:00] Telemetry property vs.willow.source : WPF
[6f88:0005][2025-02-23T13:18:00] Telemetry property vs.willow.locale : zh-TW
[6f88:0005][2025-02-23T13:18:00] Telemetry property vs.willow.installerversion : 3.13.2069.59209
[6f88:0005][2025-02-23T13:18:00] Telemetry property vs.willow.startmethod : direct
[6f88:0005][2025-02-23T13:18:00] Telemetry property vs.willow.activityid : 8269fffa-edec-4ca2-997f-1d8cf999cdd8
[6f88:0005][2025-02-23T13:18:00] Telemetry property vs.willow.campaign :
[6f88:0005][2025-02-23T13:18:00] Telemetry property vs.willow.passive : False
[6f88:0005][2025-02-23T13:18:00] Telemetry property vs.willow.quiet : True
[6f88:0005][2025-02-23T13:18:00] Telemetry property vs.willow.processtype : ui
[6f88:0005][2025-02-23T13:18:00] Telemetry property vs.willow.force : False
[6f88:0005][2025-02-23T13:18:00] Telemetry property vs.willow.noweb : False
[6f88:0005][2025-02-23T13:18:00] Telemetry property vs.willow.iselevated : True
[6f88:0005][2025-02-23T13:18:00] Telemetry property vs.willow.issystem : False
[6f88:0005][2025-02-23T13:18:00] Telemetry property vs.willow.isadmin : True
[6f88:0001][2025-02-23T13:18:00] Navigation requested from ApplicationViewModel to MainPageViewModel
[6f88:0012][2025-02-23T13:18:03] Uri 'https://go.microsoft.com/fwlink/?linkid=857708' redirected to 'https://sendvsf
eedback2.azurewebsites.net/api'
[6f88:0004][2025-02-23T13:18:04] Telemetry property vs.setup.WorkloadOverrides.DidReceiveOverride : False
[6f88:0004][2025-02-23T13:18:04] Telemetry property vs.setup.WorkloadOverrides.RuleId : None
[6f88:0011][2025-02-23T13:18:04] Telemetry property VS.SetupEngine.ChannelUpdateDisabled : False
Warning: [6f88:0011][2025-02-23T13:18:04] Setup expects one or more channel manifest in the repository.
Warning: [6f88:0011][2025-02-23T13:18:04] Setup expects one or more channel manifest in the repository.
[6f88:0011][2025-02-23T13:18:04] Authentication Environment variable setting value: null
[6f88:0011][2025-02-23T13:18:04] Authenticate: True
[6f88:0012][2025-02-23T13:18:04] Download requested: https://download.visualstudio.microsoft.com/download/pr/6fb3e1c
1-09c2-4ead-966e-fc94d48f5b17/b597dd768879cd207c65e890c6b13611c486fbacd6ef6986798ab00257d950f0/VisualStudio.17.Relea
se.chman
[6f88:001e][2025-02-23T13:18:04] Attempting download 'https://download.visualstudio.microsoft.com/download/pr/6fb3e1
c1-09c2-4ead-966e-fc94d48f5b17/b597dd768879cd207c65e890c6b13611c486fbacd6ef6986798ab00257d950f0/VisualStudio.17.Rele
ase.chman' using engine 'WebClient'
[6f88:001e][2025-02-23T13:18:06] ManifestVerifier Result: Success
[6f88:001e][2025-02-23T13:18:06] Download of 'https://download.visualstudio.microsoft.com/download/pr/6fb3e1c1-09c2-
4ead-966e-fc94d48f5b17/b597dd768879cd207c65e890c6b13611c486fbacd6ef6986798ab00257d950f0/VisualStudio.17.Release.chma
n' succeeded using engine 'WebClient'
[6f88:000e][2025-02-23T13:18:06] Trying to remove channel manifest: C:\Users\john\AppData\Local\Microsoft\VisualStud
io\Packages\_Channels\f3d8fe5d\installChannelManifest.json
[6f88:000e][2025-02-23T13:18:06] Trying to remove product manifest: C:\Users\john\AppData\Local\Microsoft\VisualStud
io\Packages\_Channels\f3d8fe5d\install_catalog.json
[6f88:0012][2025-02-23T13:18:07] Download requested: https://go.microsoft.com/fwlink/?linkid=2066144
[6f88:001e][2025-02-23T13:18:07] Attempting download 'https://go.microsoft.com/fwlink/?linkid=2066144' using engine
'WebClient'
Warning: [6f88:000f][2025-02-23T13:18:07] No previous catalog found at 'C:\ProgramData\Microsoft\VisualStudio\Packag
es\_Instances\9cb4bf22\catalog.previous.json'
[6f88:000e][2025-02-23T13:18:07] Status changed to NoUpdate
[6f88:000f][2025-02-23T13:18:07] Telemetry property VS.SetupEngine.ChannelUpdateDisabled : False
Warning: [6f88:000f][2025-02-23T13:18:07] Didn't find any channel feed.
[6f88:0009][2025-02-23T13:18:07] Setup Engine v3.13.2069, Microsoft Windows NT 10.0.19044.0
[6f88:0009][2025-02-23T13:18:07] Command line: "C:\Program Files (x86)\Microsoft Visual Studio\Installer\setup.exe"
modify --installPath "C:\Program Files (x86)\Microsoft Visual Studio\2022\BuildTools" --includeRecommended --noresta
rt --quiet --add Microsoft.VisualStudio.Workload.VCTools
[6f88:0009][2025-02-23T13:18:07] Loading packages for instance '9cb4bf22'
[6f88:001e][2025-02-23T13:18:08] Uri 'https://go.microsoft.com/fwlink/?linkid=2066144' redirected to 'https://newsfe
ed.visualstudio.microsoft.com/news/vs'
[6f88:001e][2025-02-23T13:18:08] Download of 'https://go.microsoft.com/fwlink/?linkid=2066144' succeeded using engin
e 'WebClient'
[6f88:0009][2025-02-23T13:18:09] DependencyComparer was created without a productArch. productArch will be ignored f
or dependency comparison.
[6f88:0009][2025-02-23T13:18:09] DependencyComparer was created without a productArch. productArch will be ignored f
or dependency comparison.
[6f88:0009][2025-02-23T13:18:09] DependencyComparer was created without a productArch. productArch will be ignored f
or dependency comparison.
[6f88:0009][2025-02-23T13:18:09] DependencyComparer was created without a productArch. productArch will be ignored f
or dependency comparison.
[6f88:0009][2025-02-23T13:18:09] DependencyComparer was created without a productArch. productArch will be ignored f
or dependency comparison.
[6f88:0009][2025-02-23T13:18:09] DependencyComparer was created without a productArch. productArch will be ignored f
or dependency comparison.
[6f88:0009][2025-02-23T13:18:09] DependencyComparer was created without a productArch. productArch will be ignored f
or dependency comparison.
[6f88:0009][2025-02-23T13:18:09] DependencyComparer was created without a productArch. productArch will be ignored f
or dependency comparison.
[6f88:0009][2025-02-23T13:18:09] DependencyComparer was created without a productArch. productArch will be ignored f
or dependency comparison.
[6f88:0009][2025-02-23T13:18:09] DependencyComparer was created without a productArch. productArch will be ignored f
or dependency comparison.
[6f88:0009][2025-02-23T13:18:09] DependencyComparer was created without a productArch. productArch will be ignored f
or dependency comparison.
[6f88:0009][2025-02-23T13:18:09] DependencyComparer was created without a productArch. productArch will be ignored f
or dependency comparison.
[6f88:0009][2025-02-23T13:18:09] DependencyComparer was created without a productArch. productArch will be ignored f
or dependency comparison.
[6f88:0009][2025-02-23T13:18:09] DependencyComparer was created without a productArch. productArch will be ignored f
or dependency comparison.
[6f88:0009][2025-02-23T13:18:09] DependencyComparer was created without a productArch. productArch will be ignored f
or dependency comparison.
[6f88:0009][2025-02-23T13:18:09] DependencyComparer was created without a productArch. productArch will be ignored f
or dependency comparison.
[6f88:0009][2025-02-23T13:18:09] DependencyComparer was created without a productArch. productArch will be ignored f
or dependency comparison.
[6f88:0009][2025-02-23T13:18:09] DependencyComparer was created without a productArch. productArch will be ignored f
or dependency comparison.
[6f88:0009][2025-02-23T13:18:09] DependencyComparer was created without a productArch. productArch will be ignored f
or dependency comparison.
[6f88:0009][2025-02-23T13:18:09] DependencyComparer was created without a productArch. productArch will be ignored f
or dependency comparison.
[6f88:0009][2025-02-23T13:18:09] DependencyComparer was created without a productArch. productArch will be ignored f
or dependency comparison.
[6f88:0009][2025-02-23T13:18:09] DependencyComparer was created without a productArch. productArch will be ignored f
or dependency comparison.
[6f88:0009][2025-02-23T13:18:09] DependencyComparer was created without a productArch. productArch will be ignored f
or dependency comparison.
[6f88:0009][2025-02-23T13:18:09] DependencyComparer was created without a productArch. productArch will be ignored f
or dependency comparison.
[6f88:0009][2025-02-23T13:18:09] DependencyComparer was created without a productArch. productArch will be ignored f
or dependency comparison.
[6f88:0009][2025-02-23T13:18:09] DependencyComparer was created without a productArch. productArch will be ignored f
or dependency comparison.
[6f88:0009][2025-02-23T13:18:10] Loaded existing instance for product 'Microsoft.VisualStudio.Product.BuildTools,ver
sion=17.13.35818.85'
[6f88:0009][2025-02-23T13:18:10] Telemetry property VS.SetupEngine.Id : VisualStudio/17.13.1+35818.85
[6f88:0009][2025-02-23T13:18:10] Telemetry property VS.SetupEngine.Branch : d17.13
[6f88:0009][2025-02-23T13:18:10] Telemetry property VS.SetupEngine.BuildNumber : 17.13.35818.85
[6f88:0009][2025-02-23T13:18:10] Telemetry property VS.SetupEngine.InstanceId : 9cb4bf22
[6f88:0009][2025-02-23T13:18:10] Telemetry property VS.SetupEngine.EngineVersion : 3.13.2069.59209
[6f88:0009][2025-02-23T13:18:10] Package Microsoft.Windows.UniversalCRT.Msu.7 is not applicable: 目前的作業系統版本
'10.0.19044.0' 不在支援的版本範圍 '[6.1,6.2)' 內。
[6f88:0009][2025-02-23T13:18:10] Package Microsoft.Windows.UniversalCRT.Msu.8 is not applicable: 目前的作業系統版本
'10.0.19044.0' 不在支援的版本範圍 '[6.2,6.3)' 內。
[6f88:0009][2025-02-23T13:18:10] Package Microsoft.Windows.UniversalCRT.Msu.81 is not applicable: 目前的作業系統版本 '10.0.19044.0' 不在支援的版本範圍 '[6.3,6
.4)' 內。
[6f88:0009][2025-02-23T13:18:10] Package Microsoft.VisualStudio.Debugger.DbgHelp.Win8 is not applicable: 目前的作業
系統版本 '10.0.19044.0' 不在支援的版本範圍 '[6.1,6.3]' 內。
[6f88:0009][2025-02-23T13:18:10] Package Microsoft.VisualStudio.Debugger.Remote.DbgHelp.Win8 is not applicable: 目前的作業系統版本 '10.0.19044.0' 不在支援的版
本範圍 '[6.1,6.3]' 內。
[6f88:0009][2025-02-23T13:18:10] Package Microsoft.VisualStudio.Debugger.Remote.DbgHelp.Win8 is not applicable: 目前的作業系統版本 '10.0.19044.0' 不在支援的版
本範圍 '[6.1,6.3]' 內。
[6f88:0009][2025-02-23T13:18:10] Package Microsoft.Net.4.8.FullRedist is not applicable: 目前的作業系統版本 '10.0.19044.0' 不在支援的版本範圍 '[6.1.1,10.0.177
63]' 內。
[6f88:0009][2025-02-23T13:18:10] Package Microsoft.Net.4.8.FullRedist.Resources is not applicable: 目前的作業系統版
本 '10.0.19044.0' 不在支援的版本範圍 '[6.1.1,10.0.19042)' 內。
[6f88:0009][2025-02-23T13:18:10] Package Microsoft.Net.4.8.KB5003306 is not applicable: 目前的作業系統版本 '10.0.190
44.0' 不在支援的版本範圍 '[10.0.18362,10.0.18363]' 內。
[6f88:0009][2025-02-23T13:18:10] Package Microsoft.Net.4.8.KB5003304 is not applicable: 目前的作業系統版本 '10.0.19044.0' 不在支援的版本範圍 '[10.0.19041,10.0
.19042)' 內。
[6f88:0009][2025-02-23T13:18:10] Package Microsoft.VisualStudio.NuGet.PowershellBindingRedirect is not applicable:
目前的作業系統版本 '10.0.19044.0' 不在支援的版本範圍 '[6.1,6.2)' 內。
[6f88:0009][2025-02-23T13:18:10] Package Microsoft.Windows.UniversalCRT.Msu.7 is not applicable: 目前的作業系統版本
'10.0.19044.0' 不在支援的版本範圍 '[6.1,6.2)' 內。
[6f88:0009][2025-02-23T13:18:10] Package Microsoft.Windows.UniversalCRT.Msu.8 is not applicable: 目前的作業系統版本
'10.0.19044.0' 不在支援的版本範圍 '[6.2,6.3)' 內。
[6f88:0009][2025-02-23T13:18:10] Package Microsoft.Windows.UniversalCRT.Msu.81 is not applicable: 目前的作業系統版本 '10.0.19044.0' 不在支援的版本範圍 '[6.3,6
.4)' 內。
[6f88:0009][2025-02-23T13:18:10] Package Microsoft.VisualStudio.Debugger.DbgHelp.Win8 is not applicable: 目前的作業
系統版本 '10.0.19044.0' 不在支援的版本範圍 '[6.1,6.3]' 內。
[6f88:0009][2025-02-23T13:18:10] Package Microsoft.VisualStudio.Debugger.Remote.DbgHelp.Win8 is not applicable: 目前的作業系統版本 '10.0.19044.0' 不在支援的版
本範圍 '[6.1,6.3]' 內。
[6f88:0009][2025-02-23T13:18:10] Package Microsoft.VisualStudio.Debugger.Remote.DbgHelp.Win8 is not applicable: 目前的作業系統版本 '10.0.19044.0' 不在支援的版
本範圍 '[6.1,6.3]' 內。
[6f88:0009][2025-02-23T13:18:10] Package Microsoft.Net.4.8.FullRedist is not applicable: 目前的作業系統版本 '10.0.19044.0' 不在支援的版本範圍 '[6.1.1,10.0.177
63]' 內。
[6f88:0009][2025-02-23T13:18:10] Package Microsoft.Net.4.8.FullRedist.Resources is not applicable: 目前的作業系統版
本 '10.0.19044.0' 不在支援的版本範圍 '[6.1.1,10.0.19042)' 內。
[6f88:0009][2025-02-23T13:18:10] Package Microsoft.Net.4.8.KB5003306 is not applicable: 目前的作業系統版本 '10.0.190
44.0' 不在支援的版本範圍 '[10.0.18362,10.0.18363]' 內。
[6f88:0009][2025-02-23T13:18:10] Package Microsoft.Net.4.8.KB5003304 is not applicable: 目前的作業系統版本 '10.0.19044.0' 不在支援的版本範圍 '[10.0.19041,10.0
.19042)' 內。
[6f88:0009][2025-02-23T13:18:10] Package Microsoft.VisualStudio.NuGet.PowershellBindingRedirect is not applicable:
目前的作業系統版本 '10.0.19044.0' 不在支援的版本範圍 '[6.1,6.2)' 內。
Warning: [6f88:0009][2025-02-23T13:18:10] No previous catalog found at 'C:\ProgramData\Microsoft\VisualStudio\Packag
es\_Instances\9cb4bf22\catalog.previous.json'
[6f88:0009][2025-02-23T13:18:10] Adding packages from --add
[6f88:0008][2025-02-23T13:18:10] Planning graph selection
[6f88:0008][2025-02-23T13:18:10] Graph.PlanSelection completed in 5ms
[6f88:0008][2025-02-23T13:18:10] Building the required chain
[6f88:0008][2025-02-23T13:18:10] Committing graph selection
[6f88:0008][2025-02-23T13:18:10] Graph.CommitSelection completed in 3ms
[6f88:0008][2025-02-23T13:18:10] Updating graph selection
[6f88:0008][2025-02-23T13:18:10] Graph.UpdateSelection completed in 12ms
[6f88:0008][2025-02-23T13:18:10] Notifying VMs with the updated selections: GroupSelected - Microsoft.VisualStudio.W
orkload.VCTools Microsoft.VisualStudio.Component.VC.CoreBuildTools Microsoft.VisualStudio.Component.VC.Tools.x86.x64
 Microsoft.VisualStudio.Component.VC.Redist.14.Latest Microsoft.VisualStudio.Component.Windows11SDK.22621 Microsoft.
VisualStudio.Component.VC.CMake.Project Microsoft.VisualStudio.Component.TestTools.BuildTools Microsoft.VisualStudio
.Component.VC.ASAN Microsoft.VisualStudio.ComponentGroup.NativeDesktop.Core Microsoft.VisualStudio.Component.Vcpkg M
icrosoft.VisualStudio.Component.Windows10SDK Microsoft.VisualStudio.Component.TextTemplating Microsoft.VisualStudio.
Component.VC.CoreIde
[6f88:000f][2025-02-23T13:18:11] Extension signature validation returned 0 unsigned extensions and 0 invalidly signe
d extensions.
[6f88:000f][2025-02-23T13:18:11] No restart manager available. Assuming no reboot required for instance state.
[6f88:000f][2025-02-23T13:18:11] No restart manager available. Assuming no reboot required for instance state.
[6f88:000e][2025-02-23T13:18:11] ManifestVerifier Result: Success
[6f88:000e][2025-02-23T13:18:11] Creating a UnelevatedProductModifier to modify the following packages: Microsoft.Vi
sualStudio.Product.BuildTools Microsoft.VisualStudio.Workload.MSBuildTools Microsoft.VisualStudio.Workload.VCTools M
icrosoft.VisualStudio.Component.Roslyn.Compiler Microsoft.Component.MSBuild Microsoft.VisualStudio.Component.CoreBui
ldTools Microsoft.VisualStudio.Component.Windows10SDK Microsoft.VisualStudio.Component.VC.CoreBuildTools Microsoft.V
   於 System.Runtime.CompilerServices.TaskAwaiter.HandleNonSuccessAndDebuggerNotification(Task task)
   於 Microsoft.VisualStudio.Setup.Download.DownloadManagerAuthenticationProxy.<MungeUriAsync>d__28.MoveNext()
[6f88:001e][2025-02-23T13:18:11] Download requested: https://aka.ms/vs/channels
   於 System.Runtime.CompilerServices.TaskAwaiter.HandleNonSuccessAndDebuggerNotification(Task task)
   於 Microsoft.VisualStudio.Setup.Download.DownloadManagerAuthenticationProxy.<MungeUriAsync>d__28.MoveNext()
[6f88:001e][2025-02-23T13:18:11] Download requested: https://aka.ms/vs/channels
Warning: [6f88:001f][2025-02-23T13:18:11] Failed to download channels file from https://aka.ms/vs/channels: 工作已取消。
[6f88:0020][2025-02-23T13:18:11] WebClient error 'RequestCanceled' with 'https://aka.ms/vs/installer/latest/feed' -
ExecuteWithRetryAsync failed along with a cancellation request
Warning: [6f88:0020][2025-02-23T13:18:11] Failed to get the HttpWebResponse while invoking a HEAD request against https://aka.ms/vs/installer/latest/feed:System.OperationCanceledException: 已取消作業。
   於 System.Threading.CancellationToken.ThrowOperationCanceledException()
   於 System.Threading.CancellationToken.ThrowIfCancellationRequested()
   於 Microsoft.VisualStudio.Setup.Download.WebRequestService.<>c__DisplayClass9_0.<ExecuteWithRetryAsync>b__0(Object _)
   於 System.Threading.Tasks.Task`1.InnerInvoke()
   於 System.Threading.Tasks.Task.Execute()
--- 先前擲回例外狀況之位置中的堆疊追蹤結尾 ---
   於 System.Runtime.ExceptionServices.ExceptionDispatchInfo.Throw()
   於 System.Runtime.CompilerServices.TaskAwaiter.HandleNonSuccessAndDebuggerNotification(Task task)
   於 Microsoft.VisualStudio.Setup.Download.DownloadManagerAuthenticationProxy.<MungeUriAsync>d__28.MoveNext()
[6f88:0020][2025-02-23T13:18:11] Download requested: https://aka.ms/vs/installer/latest/feed
Warning: [6f88:0021][2025-02-23T13:18:11] Failed to update the latest installer feed 工作已取消。
Warning: [6f88:0021][2025-02-23T13:18:11] Didn't find any channel feed.
[6f88:0003][2025-02-23T13:18:11] Status changed to NoUpdate
Warning: [6f88:0005][2025-02-23T13:18:11] Didn't find any channel feed.
Warning: [6f88:0004][2025-02-23T13:18:11] Didn't find any channel feed.
[6f88:0011][2025-02-23T13:18:12] Authenticode verification returned 0x00000000 for path: C:\Program Files (x86)\Microsoft Visual Studio\Installer\setup.exe.
Chocolatey timed out waiting for the command to finish. The timeout
 specified (or the default value) was '2700' seconds. Perhaps try a
 higher `--execution-timeout`? See `choco -h` for details.
  visualstudio2022-workload-vctools may be able to be automatically uninstalled.
The install of visualstudio2022-workload-vctools was NOT successful.
Error while running 'C:\ProgramData\chocolatey\lib\visualstudio2022-workload-vctools\tools\ChocolateyInstall.ps1'.
 See log for details.
```
