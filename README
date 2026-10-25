# Smart Rack Inventory System

Smart Rack is a browser-based inventory dashboard for organizing components, tools, and supplies by their physical storage location. It combines an inventory list with a visual storage manager, making it easier to see where items belong and manage the layout of a rack.

The website uses an existing cloud spreadsheet backend for inventory and storage data. Its interface refers to this service as **Cloud**.

## Storage hierarchy

```text
Shelf
└── Compartment
    └── Box
        └── Slot
```

For example, a component can be stored in **Shelf 1 → Compartment A → Box 1 → Slot 3**.

## Features

- Inventory dashboard with item names, categories, quantities, and storage locations.
- Search and category filters to help find items.
- Add and edit inventory items through the cloud endpoint.
- Visual Storage Structure Manager for shelves, compartments, boxes, and slots.
- Add shelves, compartments, boxes, and slots.
- Delete the selected shelf or compartment, including its descendant layout entries.
- Draw compartment and slot boundaries on an interactive canvas.
- Box dimensions and an area allocation display.
- Cloud layout saves with a fetch-back check before reporting success.
- Light and dark themes, with a responsive layout for different screen sizes.

## Using the storage manager

1. Open **Storage Structure Manager**.
2. Select a shelf or choose **+ Add Shelf**.
3. Select a compartment or choose **+ Add Compartment**.
4. Choose **+ Add Box** to create a container in that compartment.
5. Open a box using **Canvas →**, then choose **+ Add Slot** or draw a slot boundary.
6. Enter the requested details and select **Save**.
7. Wait for the cloud save confirmation before refreshing or closing the page.

To delete a location, select it and choose **Delete Shelf** or **Delete Compartment**. Confirm the deletion when prompted. Move any inventory items out of the location first. The final shelf cannot be deleted.

## Cloud storage

The existing cloud spreadsheet is the source of truth for the layout. Refreshing the website fetches the saved cloud structure; an older browser cache does not override it.

Layout changes use the existing Apps Script endpoint configured by `GOOGLE_SCRIPT_URL` in `index.html`. The backend must support:

- A GET response containing `items` and `structure`.
- A structure-only POST with the following format:

```json
{
  "type": "structure",
  "data": {
    "shelves": [],
    "compartments": {},
    "boxes": {},
    "slots": {}
  }
}
```

The existing backend stores this layout in `Structure!A2`. Structure-only saves leave inventory rows untouched. After submitting a layout, the website fetches it again to verify the saved result. If verification fails, it displays an error instead of claiming the change was saved.

## GitHub Pages deployment

The website is contained in a single `index.html` file, including its styles and JavaScript. No build step or frontend package installation is required.

Upload the updated `index.html` to the directory published by your existing GitHub Pages configuration. You can also upload this `README.md` as project documentation. The test file is not required for hosting.

Keep the configured cloud endpoint unless you are intentionally connecting the app to another backend. The existing deployed backend must support the requests described above.

## Files

| File | Purpose |
| --- | --- |
| `index.html` | Complete website: interface, styles, and cloud requests. |
| `README.md` | Project description and usage instructions. |
| `tests/storage-persistence.cjs` | Local checks for cloud layout saving, refresh behavior, and deletion. |

## Local verification

With Node.js installed, run:

```sh
node tests/storage-persistence.cjs
```

The test also reads the existing backend source at `../google_apps_script.gs`, so retain the original project folder layout when running it. It uses simulated cloud storage and does not change the live spreadsheet.

## Hardware integration

The interface also contains ESP8266 rack controls and item pick/return workflows. Physical LED operation requires the compatible device and firmware. Some inventory actions use device endpoints such as `/inventory` or `/fetch`; those endpoints are not provided by GitHub Pages itself. Cloud layout saving and fetching use the configured cloud endpoint directly.

