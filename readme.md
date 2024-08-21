## Using Microsoft Word (.docx) with Git

Inspired by:

- [Integrate git diffs with word docx files](https://github.com/vigente/gerardus/wiki/Integrate-git-diffs-with-word-docx-files)

### Limitation of .docx Files

Although we can use `git` to track changes in almost any file, including binary files like `.docx` files, it is difficult to visualize the changes made to these files.

I’ve put together simple instructions that you can use to visualize changes using `git diffs`.

### What Does This Do?

- Add pre-commit and post-commit hooks to keep a Markdown (.md) copy of all `.docx` files inside this repository. This allows you to use `git diffs` to show the changes in the document, which can be viewed in the terminal, on the repository's commit page, in emails, or in other places that support `git diffs`.

### Requirements

- You must install Pandoc (https://pandoc.org/) on your machine. Run `pandoc -v` in your terminal to check if it is available.

### Usage

1. Clone this repository:

   It is recommended to include `--depth=1` so your new repository will have a fresh history.

   Replace `FOLDER_NAME` with your directory name:

   ```bash
   git clone --depth=1 https://github.com/farhan-syah/git-docx.git FOLDER_NAME
   cd FOLDER_NAME
   ```

2. Copy the hooks into the `.git/hooks` directory:

   ```bash
   cp -R hooks/* .git/hooks/
   ```

3. Update the local Git configuration to enable Pandoc-based diffs for `.docx` files and add a useful alias for word-diff:

   Run the following commands inside the repository to update the local Git configuration:

   ```bash
   git config diff.pandoc.textconv "pandoc --to=markdown"
   git config diff.pandoc.prompt false
   git config alias.wdiff "diff --word-diff=color --unified=1"
   ```

   These settings will:

   - Convert `.docx` files to Markdown when generating diffs.
   - Disable the prompt when using Pandoc for diffs.
   - Create a `wdiff` alias for word-diff mode with colored output.

4. Create or modify the `.docx` files using proper `.docx` software, like Microsoft Word or LibreOffice.

   **WARNING:** Do not create or modify the `.docx` files using terminal or common text editors that have no `.docx` support, as it will break the file and `pandoc` will fail to read it. Remember, `.docx` is a binary file and not a normal text file.

   For UNSTAGED changes, you can view the difference directly by using `git wdiff`. Please note that you'll see empty file when running this command on staged or commited files.:

   ```bash
   git wdiff FILENAME.docx
   ```

   or you can simpy use `git gui blame` for both STAGED or UNSTAGED:

   ```bash
   git gui blame FILENAME.docx
   ```

5. Stage and commit the files:

   ```bash
   git add FILENAME.docx
   git commit -m "Amend FILENAME.docx"
   ```

   When you stage the documents and create a new commit, a copy of the files in `.md` will be created/updated, and you can utilize git tools to view the changes.

### View File Changes

To view the history of changes made to the `.md` files (which are the Markdown copies of your `.docx` files), you can use a few different Git commands. Here’s how:

1. **Using `git log` to View the History**

   The `git log` command is the most straightforward way to view the commit history of a file:

   ```bash
   git log -- FILENAME.md
   ```

   Replace `FILENAME.md` with the name of the Markdown file you want to inspect. This command will show the commit history for that specific file.

2. **Viewing Differences with `git log -p`**

   If you want to see the actual changes made to the file in each commit, you can use:

   ```bash
   git log -p -- FILENAME.md
   ```

   This will show you the commit messages along with the differences introduced in each commit.

3. **Using `git diff` to Compare Versions**

   If you know the specific commits or branches you want to compare, you can use `git diff`:

   ```bash
   git diff COMMIT1 COMMIT2 -- FILENAME.md
   ```

   Replace `COMMIT1` and `COMMIT2` with the commit hashes or branch names you want to compare. This will show you the changes between the two versions of the file.

4. **Using `git wdiff` for Word-Level Diffs**

   If you set up the `wdiff` alias (as previously discussed), you can use it to view word-level differences in the `.docx` files:

   ```bash
   git wdiff COMMIT1 COMMIT2 -- FILENAME.docx
   ```

   This will give you a more granular view of the changes, highlighting individual word changes within the lines.

5. **Viewing History in a GUI**

   If you prefer a graphical interface, you can use tools like `gitk`, `GitHub Desktop`, `VSCode Git Extensions` or any Git client that supports visual diffs. For example:

   ```bash
   gitk FILENAME.md
   ```

   This command opens a graphical history viewer for the specific file, showing commits and diffs visually.

### Summary of Commands:

- View commit history: `git log -- FILENAME.md`
- View commit history with diffs: `git log -p -- FILENAME.md`
- Compare two versions: `git diff COMMIT1 COMMIT2 -- FILENAME.md`
- Word-level diffs: `git wdiff COMMIT1 COMMIT2 -- FILENAME.docx`
- GUI view (gitk): `gitk FILENAME.docx` or `gitk FILENAME.md`
- GUI view (git gui): `git gui blame FILENAME.docx` or `git gui blame FILENAME.md`

These commands will help you track and inspect the history of changes made to your Markdown files in the repository.
