---
published: true
---
## Exception to `set -e` in bash

```sh
set +e
# your command which would fail
set -e

```

or

```sh
your_command_which_would_fail || true
```
https://stackoverflow.com/questions/48079029/exception-to-set-e-in-bash

---

# cd - 解析

#### You:
解釋 
cd - >/dev/null
cd - >/dev/null || error "Cannot return to previous directory"

#### ChatGPT:
這兩行是用來切換回上一個目錄並隱藏輸出，如果失敗則顯示錯誤訊息。以下是逐步解析：

### **第一行**

```sh
cd - >/dev/null
```

- `cd -`：這個指令會切換到上一個目錄（類似於 `git checkout -` 切換回前一個分支）。
- `>/dev/null`：將標準輸出重定向到 `/dev/null`，這樣終端就不會顯示切換過去的目錄名稱。

### **第二行**

```sh
cd - >/dev/null || error "Cannot return to previous directory"
```

- `cd - >/dev/null`：同上，嘗試切換到上一個目錄並隱藏輸出。
- `||`：如果 `cd -` 失敗（返回非零狀態碼），則執行 `error "Cannot return to previous directory"`。
- `error "Cannot return to previous directory"`：
  - 這裡假設 `error` 是一個自定義函式，用來輸出錯誤信息並可能終止腳本執行。
  - 如果 `error` 沒有定義，則會導致「command not found」錯誤，應該改用 `echo` 或 `exit`，例如：
    ```sh
    cd - >/dev/null || { echo "Cannot return to previous directory"; exit 1; }
    ```

### **總結**

這段程式碼的作用是**嘗試返回上一個目錄，並隱藏輸出，如果失敗則報錯**。這在需要切換目錄但不想干擾終端輸出的情境下很有用。


#### You:
有 set -o pipefail
那當cd - >/dev/null出錯時，還會執行後半段的程式|| { echo "Cannot return to previous directory"; exit 1; }嗎

#### ChatGPT:
是的，`cd - >/dev/null || { echo "Cannot return to previous directory"; exit 1; }` 仍然會執行後半段的 `{}`，即錯誤處理部分，即使 `set -o pipefail` 已啟用。

### **解析**

1. **`set -o pipefail` 的作用**
   - 這個選項只影響**管道（`|`）的行為**，使得**整個管道中的任何一個命令失敗時，整個管道的返回值就會是失敗**。
   - 但這裡的 `cd - >/dev/null` **沒有使用管道（`|`）**，只是重定向輸出，所以 **`set -o pipefail` 不會影響它的行為**。

2. **`cd - >/dev/null` 的行為**
   - `cd -` 可能失敗，例如：
     - 如果沒有上一個目錄可返回（如剛開啟終端時）。
     - 目標目錄被刪除或無權存取。
   - 如果 `cd -` 失敗，它會返回非零的退出狀態碼（例如 `1`）。
   - `>/dev/null` 只是丟棄標準輸出，不影響 `cd -` 是否成功。

3. **`||` 操作符的作用**
   - `||` 只在**左側命令失敗（返回非零狀態碼）時才執行右側命令**。
   - `cd -` 若失敗，則 `{ echo "Cannot return to previous directory"; exit 1; }` 會被執行。
   - `cd -` 若成功，則 `{}` 內的命令不會執行。

### **結論**

**`set -o pipefail` 不會影響這段程式碼的行為**，如果 `cd - >/dev/null` 失敗，`||` 右側的錯誤處理仍然會執行。


#### You:
那set -e呢

#### ChatGPT:
如果啟用了 `set -e`，**`cd - >/dev/null` 失敗時，整個腳本會直接終止，而不會執行 `|| { echo "Cannot return to previous directory"; exit 1; }`**。

---

### **`set -e` 的行為**

- `set -e` 會讓**腳本在任一命令返回非零狀態碼時立即終止**（除非該命令的返回值被 `if`、`&&`、`||` 或某些其他控制結構處理）。
- 在 `cd - >/dev/null` 失敗時，因為 `cd -` 返回非零狀態，而它不是 `if` 或 `||` 這類結構的直接一部分，**腳本會立即終止**，導致後面的 `||` 不會執行。

---

### **如何讓錯誤處理仍然執行**

如果希望 `set -e` 不影響 `cd -` 失敗時的錯誤處理，可以改用 `if`：

```sh
if ! cd - >/dev/null; then
    echo "Cannot return to previous directory"
    exit 1
fi
```

這樣 `cd -` 失敗時，腳本不會因 `set -e` 而直接終止，而是會進入 `if` 內部執行錯誤處理。

---

### **結論**

當 `set -e` 啟用時：

- **原本的 `cd - >/dev/null || { echo "..."; exit 1; }` 方式不會執行 `{}`，因為 `cd -` 失敗時腳本會直接終止**。
- **建議改用 `if ! cd -` 來確保錯誤處理仍然執行**。

---

To view keyboard shortcuts, press question mark

View keyboard shortcuts
--------------------------------------------------------------------------


Grok 3

beta

See new posts

""" #!/usr/bin/env bashExit on errors and pipe failuresset -e set -o pipefailConfigurationBUILD_DIR="build" CONTENT_DIR="src/content" ASSETS_DIR="src/assets" PROCS_NUM=4 LOG_FILE="build.log"Colors for outputRED='\033[0;31m' GREEN='\033[0;32m' NC='\033[0m' # No ColorLogging functionlog() { local timestamp=$(date '+%Y-%m-%d %H:%M:%S') echo "[$timestamp] $1" | tee -a "$LOG_FILE" }Error handling functionerror() { log "${RED}ERROR: $1${NC}" exit 1 }Start timerstart=$(date +%s)Initialize build directorylog "Initializing build directory..." mkdir -p "$BUILD_DIR" || error "Failed to create build directory"Clean specific files from build directorylog "Cleaning old HTML files..." find "$CONTENT_DIR" -type f

`-name "*.htm" -o -name "*.html"`

-printf "%P\0" | xargs -0 -n1 -P "$PROCS_NUM" -I {} bash -c 'rm -fv "'"$BUILD_DIR"'/{}" >> "'"$LOG_FILE"'"' || error "Failed to clean old files"Remove orphaned files from buildlog "Removing orphaned files..." cd "$BUILD_DIR" || error "Cannot change to build directory" IFS=$'\n'; set -f for filename in $(find . -type f); do filepath="${filename#./}" if [[ ! -f "../$CONTENT_DIR/$filepath" && ! -f "../$ASSETS_DIR/$filepath" ]]; then rm -fv "$filepath" >> "../$LOG_FILE" || error "Failed to remove $filepath" fi done unset IFS; set +f cd - >/dev/null || error "Cannot return to previous directory"Generate YAML cachelog "Generating YAML cache..." node yamls.js > yamls.cache || error "Failed to generate YAML cache"Compile content fileslog "Compiling content files..." find "$CONTENT_DIR" -type f

`-name "*.htm" -o -name "*.html"`

-printf "%P\0" | xargs -0 -n1 -P "$PROCS_NUM" -I {} bash -c 'node compile.js "'"$CONTENT_DIR"'/{}" "'"$BUILD_DIR"'/{}" || echo "Failed to compile {}" >> "'"$LOG_FILE"'"' || error "Compilation failed"Sync assetslog "Syncing assets..." cd "$ASSETS_DIR" || error "Cannot change to assets directory" IFS=$'\n'; set -f for filename in $(find . -type f); do filepath="${filename#./}" target_file="../../$BUILD_DIR/$filepath" if [[ ! -f "$target_file" || "$target_file" -ot "$filepath" ]]; then mkdir -p "$(dirname "$target_file")" || error "Failed to create directory for $filepath" cp -fv "$filepath" "$target_file" >> "../../$LOG_FILE" || error "Failed to copy $filepath" fi done unset IFS; set +f cd - >/dev/null || error "Cannot return to previous directory"Clean empty directorieslog "Cleaning empty directories..." find "$BUILD_DIR" -type d -empty -delete -print >> "$LOG_FILE" || error "Failed to clean empty directories"End timer and reportend=$(date +%s) elapsed=$((end-start)) log "${GREEN}Build completed successfully${NC}" log "Elapsed Time: $elapsed seconds"exit 0 """Error Handling kind of useless if I have set -e

Thought for

32s

Collapse details

Let's break down the script and see how error handling is managed, especially with set -e in place.

First, set -e ensures that the script exits immediately if any command fails (returns a non-zero exit status). This is a common way to handle errors in bash scripts, as it prevents the script from continuing after an error occurs.

Now, looking at the script, there are several places where error handling is explicitly managed using the error function, which logs an error message and exits with a non-zero status. For example:

bash

```
mkdir -p "$BUILD_DIR" || error "Failed to create build directory"
```

Here, if mkdir -p "$BUILD_DIR" fails, the error function will be called, logging the error and exiting the script.

Similarly, in other parts of the script, such as:

bash

```
find "$CONTENT_DIR" -type f \( -name "*.htm" -o -name "*.html" \) -printf "%P\0" |
    xargs -0 -n1 -P "$PROCS_NUM" -I {} bash -c 'rm -fv "'"$BUILD_DIR"'/{}" >> "'"$LOG_FILE"'"' ||
    error "Failed to clean old files"
```

If the find or xargs command fails, the error function will be invoked.

However, since set -e is already in place, the script would exit on any command failure anyway. So, is the explicit error handling with the error function redundant?

Not necessarily. The error function provides a way to log a specific error message before exiting, which can be helpful for debugging. Without it, the script would still exit on error due to set -e, but you might not get a clear error message indicating what went wrong.

For instance, if mkdir -p "$BUILD_DIR" fails, with set -e, the script would exit, but without the error function, you might not know exactly why it failed unless you check the exit status or look at the command's output.

Moreover, in the xargs command, the bash -c script is run in a subshell, and set -e might not propagate to subshells in the same way. However, in this case, since the xargs command is piped from find, and set -e is set in the main script, if find or xargs fails, the script should still exit due to set -e.

But wait, actually, in the context of pipes, set -e behaves a bit differently. By default, in a pipeline, only the exit status of the last command in the pipeline is considered for set -e. However, since the script also has set -o pipefail, which causes a pipeline to return the exit status of the last command that failed, or 0 if all commands succeeded, this ensures that if any command in the pipeline fails, the script will exit.

So, with set -e and set -o pipefail, the script will exit if any command in a pipeline fails.

Given that, the explicit || error "message" might seem redundant, but it still serves a purpose: it allows for logging a specific error message before exiting, which can be more informative than just exiting with a generic error.

Additionally, in some cases, you might want to handle errors differently, perhaps by continuing execution or performing some cleanup, but in this script, since the intention is to exit on any error, the error function is consistent with that behavior.

Another point to consider is that in the xargs command, the bash -c script is run in parallel with -P "$PROCS_NUM", meaning multiple instances are running concurrently. If one of these subshells fails, it might not directly cause the main script to exit, but since xargs will return a non-zero exit status if any of the commands it runs fail, and with set -e, the main script should still exit.

Wait, actually, xargs will continue running all commands even if some fail, and its exit status will be 0 if all commands succeeded, or 123 if any command failed (with --process-slot-var or similar options, but in this case, it's not specified). However, in this script, since set -e is in effect, and the xargs command is part of the main script, if xargs returns a non-zero exit status, the script should exit.

But to ensure that the script exits if any of the parallel bash -c commands fail, it's better to have the || error "message" after the xargs command, as is already done.

Furthermore, inside the bash -c script, there's an || echo "Failed to compile {}" >> "$LOG_FILE", which logs a message if the node compile.js command fails, but it doesn't exit the subshell with a non-zero status explicitly. However, since set -e is not inherited by the subshell (unless bash -e is used, but here it's bash -c), the subshell will continue even if node compile.js fails, and xargs will consider the command to have succeeded if the last command in the subshell succeeds.

Wait, in this case, the bash -c command is:

bash

```
node compile.js "$CONTENT_DIR/{}" "$BUILD_DIR/{}" || echo "Failed to compile {}" >> "$LOG_FILE"
```

So, if node compile.js fails, it will execute echo "Failed to compile {}" >> "$LOG_FILE", and since echo likely succeeds, the overall exit status of the bash -c command would be 0, meaning xargs would not detect a failure.

Therefore, to make xargs detect failures, you might want the bash -c command to exit with a non-zero status if node compile.js fails. One way to do this is to use:

bash

```
node compile.js "$CONTENT_DIR/{}" "$BUILD_DIR/{}" || { echo "Failed to compile {}" >> "$LOG_FILE"; exit 1; }
```

This way, if node compile.js fails, it logs the message and exits the subshell with status 1, which xargs can detect.

Alternatively, you could set set -e inside the bash -c script, but since it's a single command, it's simpler to use the above approach.

Wait, actually, in this specific case, since the bash -c script is just one command followed by || echo ..., and without set -e, the subshell will exit with the status of the last command executed. If node compile.js succeeds, it exits with 0. If it fails, it executes echo, which succeeds, so the subshell exits with 0. Therefore, xargs will always see a successful command.

To fix this, you can modify the bash -c command to:

bash

```
node compile.js "$CONTENT_DIR/{}" "$BUILD_DIR/{}" || { echo "Failed to compile {}" >> "$LOG_FILE"; false; }
```

Here, false will ensure the subshell exits with a non-zero status if node compile.js fails.

Alternatively, you can use:

bash

```
if ! node compile.js "$CONTENT_DIR/{}" "$BUILD_DIR/{}"; then
    echo "Failed to compile {}" >> "$LOG_FILE"
    exit 1
fi
```

But since it's a one-liner, the first approach is more concise.

Wait, actually, in the original script, the bash -c command is:

bash

```
node compile.js "'"$CONTENT_DIR"'/{}" "'"$BUILD_DIR"'/{}" || echo "Failed to compile {}" >> "'"$LOG_FILE"'"'
```

So, to make it exit with non-zero status on failure, you can change it to:

bash

```
node compile.js "'"$CONTENT_DIR"'/{}" "'"$BUILD_DIR"'/{}" || { echo "Failed to compile {}" >> "'"$LOG_FILE"'"; false; }
```

This way, if node compile.js fails, it logs the message and then executes false, causing the subshell to exit with status 1.

Then, xargs will detect that some commands failed, and since xargs is run with || error "Compilation failed", if any of the parallel commands fail, xargs will return a non-zero status, triggering the error function.

Therefore, to make the error handling more robust, you should modify the bash -c command to exit with non-zero status on failure.

Similarly, in the asset syncing part:

bash

```
cp -fv "$filepath" "$target_file" >> "../../$LOG_FILE" || error "Failed to copy $filepath"
```

Wait, actually, in the script, it's:

bash

```
cp -fv "$filepath" "$target_file" >> "../../$LOG_FILE" || error "Failed to copy $filepath"
```

But since this is inside a loop, and set -e is in effect, if cp fails, the script will exit due to set -e, but again, using || error "message" provides a specific error message.

However, in this case, since it's not in a subshell, set -e will directly handle it.

Wait, actually, in the script, the asset syncing is done in a loop:

bash

```
for filename in $(find . -type f); do
    filepath="${filename#./}"
    target_file="../../$BUILD_DIR/$filepath"
    if [[ ! -f "$target_file" || "$target_file" -ot "$filepath" ]]; then
        mkdir -p "$(dirname "$target_file")" || error "Failed to create directory for $filepath"
        cp -fv "$filepath" "$target_file" >> "../../$LOG_FILE" || error "Failed to copy $filepath"
    fi
done
```

Here, for each file, if mkdir -p or cp fails, the error function will be called, logging the specific error and exiting.

Since set -e is in effect, even without the || error "message", the script would exit on failure, but again, the error function provides a meaningful message.

Moreover, in this context, since it's not in a subshell, set -e will handle it directly.

One thing to note is that in the for loop, if a command fails and error is called, it will exit the script, so the loop won't continue for the remaining files. If you want to process all files and collect errors, you might want a different approach, but in this script, the intention seems to be to stop on the first error.

Similarly, in the cleaning part:

bash

```
for filename in $(find . -type f); do
    filepath="${filename#./}"
    if [[ ! -f "../$CONTENT_DIR/$filepath" && ! -f "../$ASSETS_DIR/$filepath" ]]; then
        rm -fv "$filepath" >> "../$LOG_FILE" || error "Failed to remove $filepath"
    fi
done
```

Again, if rm fails, it will call error and exit.

Overall, while set -e provides a safety net to exit on errors, the explicit || error "message" statements enhance the script by providing specific error messages, which can be very helpful for debugging.

However, in the case of the parallel compilation with xargs, the current implementation might not correctly handle failures in the subshells, as explained earlier. To fix that, you should modify the bash -c command to ensure it exits with a non-zero status if node compile.js fails.

Additionally, you might want to consider if there are any commands where you don't want the script to exit on failure. For example, in the cleaning phase, if removing a file fails, maybe you want to continue with other files. But in this script, it seems consistent to exit on any error.

Another consideration is that set -e can sometimes lead to unexpected behavior, especially with commands that are expected to fail occasionally, but in this build script, it's probably desirable to exit on any unexpected failure.

In summary, while set -e provides a baseline for error handling, the explicit || error "message" statements are not entirely useless; they provide valuable context about what went wrong. However, for the parallel compilation step, the error handling needs adjustment to properly propagate failures from the subshells.

So, to improve the script, you could update the xargs command for compilation to ensure that subshell failures are detected:

bash

```
find "$CONTENT_DIR" -type f \( -name "*.htm" -o -name "*.html" \) -printf "%P\0" |
    xargs -0 -n1 -P "$PROCS_NUM" -I {} bash -c 'node compile.js "'"$CONTENT_DIR"'/{}" "'"$BUILD_DIR"'/{}" || { echo "Failed to compile {}" >> "'"$LOG_FILE"'"; exit 1; }' ||
    error "Compilation failed"
```

This way, if any node compile.js command fails, the subshell exits with status 1, xargs detects the failure, and the error function is called.

Alternatively, you could collect the exit statuses and handle errors after all parallel jobs are done, but in this script, exiting on the first error seems appropriate.

Therefore, the error handling in the script is not entirely useless; it complements set -e by providing specific error messages and ensures that failures in subshells are properly handled.

Is Error Handling Useless with set -e in This Script?

In your Bash script, you've included set -e at the beginning, which causes the script to exit immediately if any command returns a non-zero exit status (i.e., fails). Additionally, you've defined an error function to log specific error messages and exit with a failure status, and you use it explicitly in many places with the || operator, like this:

bash

```
mkdir -p "$BUILD_DIR" || error "Failed to create build directory"
```

You're wondering whether this explicit error handling is redundant or "useless" given that set -e is already in place. Let's explore this by examining how error handling works in your script and whether the explicit error calls add value.

* * * * *

How set -e Works

The set -e option ensures that the script stops execution as soon as any command fails, preventing it from proceeding in an erroneous state. For example, if mkdir -p "$BUILD_DIR" fails, the script exits immediately without running subsequent commands. Additionally, you've used set -o pipefail, which extends this behavior to pipelines: if any command in a pipeline (e.g., find | xargs) fails, the pipeline's exit status reflects that failure, and set -e triggers an exit.

With this setup, set -e acts as a safety net, catching most errors automatically. So, why bother with explicit || error "message" statements?

* * * * *

The Role of the error Function

The error function in your script does two things:

1.  Logs a specific error message: It uses the log function to append a timestamped message (in red) to $LOG_FILE and echoes it to the terminal.

2.  Exits with a failure status: It calls exit 1, ensuring the script stops.

Here's an example from your script:

bash

```
mkdir -p "$BUILD_DIR" || error "Failed to create build directory"
```

If mkdir fails:

-   Without the || error part, set -e would still exit the script, but you'd only get the default error output from mkdir (if any), with no custom message in your log file.

-   With the || error part, you get a clear message in $LOG_FILE, like "[2023-10-25 14:30:00] ERROR: Failed to create build directory", making it easier to debug.

So, while set -e ensures the script exits on failure, the error function enhances this by providing context about what went wrong, which is especially useful in a build script where you need to troubleshoot issues later.

* * * * *

Is It Redundant in Simple Commands?

For straightforward commands executed in the main script (not in subshells or pipelines), such as:

bash

```
mkdir -p "$BUILD_DIR" || error "Failed to create build directory"
```

or

bash

```
node yamls.js > yamls.cache || error "Failed to generate YAML cache"
```

set -e alone would suffice to stop the script on failure. The explicit || error "message" doesn't change the script's flow---it still exits---but it adds a descriptive log entry. This isn't "useless"; it's a deliberate design choice to improve observability. Without it, you'd rely on the failing command's output (which might be cryptic or absent) and the script's abrupt termination to figure out the problem.

In loops, like this asset-syncing block:

bash

```
for filename in $(find . -type f); do
    filepath="${filename#./}"
    target_file="../../$BUILD_DIR/$filepath"
    if [[ ! -f "$target_file" || "$target_file" -ot "$filepath" ]]; then
        mkdir -p "$(dirname "$target_file")" || error "Failed to create directory for $filepath"
        cp -fv "$filepath" "$target_file" >> "../../$LOG_FILE" || error "Failed to copy $filepath"
    fi
done
```

set -e ensures the script exits if mkdir or cp fails, but the error function pinpoints which file caused the issue (e.g., "Failed to copy path/to/file"). This specificity is valuable when processing many files.

* * * * *

Complications with Pipelines and Subshells

Your script uses pipelines and parallel execution with xargs, which introduces some nuance:

1.  Pipelines with set -e and set -o pipefail\
    In the cleaning step:

    bash

    ```
    find "$CONTENT_DIR" -type f \( -name "*.htm" -o -name "*.html" \) -printf "%P\0" |
        xargs -0 -n1 -P "$PROCS_NUM" -I {} bash -c 'rm -fv "'"$BUILD_DIR"'/{}" >> "'"$LOG_FILE"'"' ||
        error "Failed to clean old files"
    ```

    Here, set -o pipefail ensures that if find or xargs fails, the pipeline's exit status is non-zero, and set -e exits the script. The || error "Failed to clean old files" logs a specific message, which remains useful for debugging. Without it, you'd know cleaning failed but not why.

2.  Parallel Compilation with xargs\
    The compilation step is trickier:

    bash

    ```
    find "$CONTENT_DIR" -type f \( -name "*.htm" -o -name "*.html" \) -printf "%P\0" |
        xargs -0 -n1 -P "$PROCS_NUM" -I {} bash -c 'node compile.js "'"$CONTENT_DIR"'/{}" "'"$BUILD_DIR"'/{}" || echo "Failed to compile {}" >> "'"$LOG_FILE"'"' ||
        error "Compilation failed"
    ```

    Here, xargs runs bash -c commands in parallel (up to $PROCS_NUM processes). Each bash -c subshell executes:

    bash

    ```
    node compile.js "$CONTENT_DIR/{}" "$BUILD_DIR/{}" || echo "Failed to compile {}" >> "$LOG_FILE"
    ```

    Problem: If node compile.js fails, the echo command runs and succeeds, so the subshell exits with status 0 (success). xargs only checks the exit status of each subshell, so it won't detect the failure unless all subshells succeed. If any subshell should signal failure, xargs returns a non-zero status (e.g., 123), triggering || error "Compilation failed".

    Issue: The current setup masks individual compilation failures because the subshell always exits with 0 (since echo succeeds). This makes the || error "Compilation failed" less effective---it might not trigger even if some files fail to compile.

    Fix: To ensure failures propagate, modify the bash -c command to exit with a non-zero status on failure:

    bash

    ```
    bash -c 'node compile.js "'"$CONTENT_DIR"'/{}" "'"$BUILD_DIR"'/{}" || { echo "Failed to compile {}" >> "'"$LOG_FILE"'"; exit 1; }'
    ```

    Now, if node compile.js fails, the subshell logs the error and exits with status 1. xargs detects this, returns a non-zero status, and || error "Compilation failed" is triggered. This aligns the explicit error handling with set -e's intent.

* * * * *

When Explicit Error Handling Adds Unique Value

Beyond logging, explicit error handling can:

-   Handle subshell quirks: As seen in the xargs compilation step, set -e doesn't apply to subshells unless explicitly set (e.g., bash -e). The || operator ensures failures are caught where set -e might not reach.

-   Allow customization: If you later decide to handle certain failures differently (e.g., retrying instead of exiting), the error function provides a hook to modify behavior without removing set -e.

-   Improve parallel execution: In parallel tasks, explicit error propagation (e.g., exit 1) ensures the main script reacts appropriately.

* * * * *

Could You Rely Solely on set -e?

Technically, yes---for most commands in the main script, set -e alone would stop execution on failure. You could remove many || error "message" instances and still have a functional script. However:

-   You'd lose detailed error messages, making debugging harder.

-   Subshells (like in xargs) require extra care to propagate errors, as set -e doesn't inherit automatically.

-   Pipelines and loops might behave unexpectedly without explicit checks.

For example, if you removed all error calls:

bash

```
mkdir -p "$BUILD_DIR"
node yamls.js > yamls.cache
```

The script would still exit on failure due to set -e, but $LOG_FILE wouldn't record why, reducing traceability.

* * * * *

Conclusion

The explicit error handling with || error "message" is not useless in your script, even with set -e. It:

1.  Enhances debugging: Specific error messages in $LOG_FILE make it easier to identify issues, especially in a build process with many steps.

2.  Complements set -e: It ensures consistent behavior across subshells and pipelines where set -e alone might fall short (e.g., the xargs compilation step).

3.  Adds robustness: It provides a clear contract that the script exits with a meaningful message on failure.

However, there's one improvement needed: fix the parallel compilation step to propagate subshell failures correctly. Update this line:

bash

```
find "$CONTENT_DIR" -type f \( -name "*.htm" -o -name "*.html" \) -printf "%P\0" |
    xargs -0 -n1 -P "$PROCS_NUM" -I {} bash -c 'node compile.js "'"$CONTENT_DIR"'/{}" "'"$BUILD_DIR"'/{}" || echo "Failed to compile {}" >> "'"$LOG_FILE"'"' ||
    error "Compilation failed"
```

to:

bash

```
find "$CONTENT_DIR" -type f \( -name "*.htm" -o -name "*.html" \) -printf "%P\0" |
    xargs -0 -n1 -P "$PROCS_NUM" -I {} bash -c 'node compile.js "'"$CONTENT_DIR"'/{}" "'"$BUILD_DIR"'/{}" || { echo "Failed to compile {}" >> "'"$LOG_FILE"'"; exit 1; }' ||
    error "Compilation failed"
```

With this change, your error handling is both effective and meaningful, working harmoniously with set -e and set -o pipefail to ensure a robust build script.

