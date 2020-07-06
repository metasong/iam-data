# monaco editor dirty
*summery* https://github.com/Microsoft/monaco-editor/issues/353*
There is model.getAlternativeVersionId():

Get the alternative version id of the model. This alternative version id is not always incremented, it will return the same values in the case of undo-redo.

So to implement a dirty indicator for example:

initialize lastSavedVersionId = model.getAlternativeVersionId()
when the user saves, save the file and record lastSavedVersionId = model.getAlternativeVersionId()
when the content changes (model.onDidChangeContent(...)), you can determine fileIsDirty = (lastSavedVersionId !== model.getAlternativeVersionId())