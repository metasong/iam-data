# monaco editor undo redo
*https://github.com/Microsoft/monaco-editor/issues/451*
 j
I would not recommend calling undo() or redo() directly on the IModel, since that will undo the edits on the text, but will not restore the cursor position correctly.

I would recommend calling editor.trigger('whatever...', 'undo') and editor.trigger('whatever...', 'redo') which will end up executing IModel.undo() or IModel.redo(), but will do so through the context of the cursor which can restore its position correctly.