# Tips
The Tips section includes more information about the element including FAQs, endpoint limitations, and general tips.

* If you do not permanently delete dropbox files, they remain in your Trash folder and generate an `UPDATED` event type if they are re-created.

* Dropbox includes a `refId` for certain APIs.  This is the actual ID of the file sent back from Dropbox.  It can be used as the `ID` with all API calls with the exception of `GET /folders/{id}/contents`.

    The `GET /folders/{id}/contents` endpoint does not support the `refId` at this time.
