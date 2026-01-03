# Walkthrough: Adoption of 'tudu'

We successfully identified, adopted (forked), and installed the orphaned CLI package `tudu`.

## 1. Finding Orphaned CLI Packages

We used the `wnpp_picker` tool to identify orphaned packages suitable for adoption.

```bash
# Fetch the latest list of orphaned packages from Debian
./wnpp_picker.sh fetch

# Analyze packages to tag them (CLI, GUI, Library, etc.)
./wnpp_picker.sh analyze

# Filter for CLI-only applications, excluding libraries and GUIs
./wnpp_picker.sh query --include cli --exclude "gui lib"
```

From the filtered results, we selected **tudu** (Hierarchical ToDo list).

## 2. Adoption (Forking)

We located the upstream repository and created a personal fork to begin maintenance.

*   **Upstream**: `meskio/tudu`
*   **Fork**: `hks-bornemann/tudu`
*   **Clone**: `/home/yamiybm/Desktop/GIT/calilegua/tudu`

```bash
# Forking the repository using GitHub CLI
gh repo fork meskio/tudu --clone
```

## 3. Installation

We installed the necessary build dependencies and compiled the application from source.

```bash
# 1. Install dependencies (ncurses library)
sudo apt-get install -y libncurses5-dev libncursesw5-dev

# 2. Configure the build environment
./configure

# 3. Compile the source code
make

# 4. Install the binary to the system (requires root)
sudo make install
```

## 4. How to Run

You can now run the program from anywhere in your terminal:

```bash
tudu
```

## 5 what is tudu?
It's a manager for hierarchical todo lists for the terminal. It's very useful for organizing big projects where a big task can be in many split into smaller tasks. 

## 6. Usage Cheat Sheet

Here are the most common keyboard shortcuts to control `tudu`:

### Navigation
-   **Arrows / j, k**: Move up/down between tasks.
-   **h**: Move left (out of a subtask).
-   **l**: Move right (into a subtask/expand).
-   **c**: Collapse/Expand children.

### Editing
-   **o**: Add a new task below.
-   **O**: Add a new task above.
-   **a**: Edit the title of task.
-   **e**: Edit the description (opens external editor).
-   **dd**: Delete task.
-   **m**: Mark task as Done/Pending.

### Saving & Quitting
-   **s**: Save changes.
-   **q**: Quit (saves automatically).
-   **Q**: Quit WITHOUT saving.
-   **?**: Show help.
