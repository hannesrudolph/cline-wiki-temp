Sometimes when using Cline to edit files, you may notice it replaces large chunks of code with placeholder comments like `// rest of code here`. This behavior occurs due to the AI model's maximum output token limit. For large files, the model may truncate portions of the code to fit within its constraints.

## Best Practices

To mitigate this issue:

1. Keep your files relatively small and modular.
2. Consider moving large constants (e.g. big strings) into separate files.

## Workaround: Using VSCode's Diff Editor

When Cline edits a file, it opens in VSCode's diff editor view. This presents two side-by-side versions of the file:

- **Left side:** Virtual document with file's original contents
- **Right side:** Updated file with Claude's changes

### Step 1: Expand the Diff View for Easier Editing

<p>
  <img src="https://github.com/user-attachments/assets/9e466992-7fc4-48cc-b5b8-19b9e27cb94a" width="900" />
</p>

1. By default, the diff view may be collapsed, showing changes merged into a single view. This can make editing more challenging.
2. To improve visibility and ease of editing, resize the window or tab to be wider. This will expand the diff view, splitting it into left (original) and right (modified) panels, making it much easier to compare and edit the changes.

### Step 2: Using the Revert Block Button

<p>
  <img src="https://github.com/user-attachments/assets/4d26653f-d755-4b8b-bf9c-823a1146594f" width="900" />
</p>

1. Locate the truncated section in the right-side document.
2. Click the "Revert Block" button in the gutter between the two views to restore the original content for that section.

### Alternative: Manual Copy and Paste

<p>
  <img src="https://github.com/user-attachments/assets/57bdf1f2-55ad-4a79-bf1a-de6a753e136d" width="900" />
</p>

If you're having trouble with the revert block button:

1. Select the missing content from the left-side document.
2. Copy the selected text.
3. Paste it into the appropriate location in the right-side document.

By using these methods, you can easily restore any content that the model may have truncated, ensuring your code remains intact. When you make these manual adjustments, the extension communicates these edits back to Cline so he knows the final state of the file before proceeding in the task.

<p>
  <img src="https://github.com/user-attachments/assets/def5e644-d4a8-4a99-a690-843579680104" width="400" />
</p>

## Support

If you've followed these steps and are still experiencing problems, please:

1. Check the [Cline GitHub Issues](https://github.com/cline/clinebot/issues) to see if others have reported similar problems
2. If not, create a new issue with details about your operating system, VSCode/Cursor version, and the steps you've tried

For additional help, join our [Discord](https://discord.gg/cline).