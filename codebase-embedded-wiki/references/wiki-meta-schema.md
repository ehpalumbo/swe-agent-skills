# Wiki Tracking Manifest (`.wiki-meta.json`)

Stored at the root of the repository, `.wiki-meta.json` records minimal tracking state needed exclusively for differential updates (`sync`) and lag checks (`lint`).

---

## Schema

| Field | Required | Type | Description |
| :--- | :---: | :--- | :--- |
| `version` | **Yes** | String | Schema version (currently `"1.0.0"`). |
| `last_sync_commit` | **Yes** | String | Full 40-character SHA of the last git commit against which the wiki was synchronized. |
| `last_sync_timestamp` | **Yes** | String | ISO-8601 timestamp of when the last sync was performed. |

---

## Example

```json
{
  "version": "1.0.0",
  "last_sync_commit": "a1b2c3d4e5f67890123456789abcdef012345678",
  "last_sync_timestamp": "2026-07-31T17:00:00Z"
}
```

---

## Usage Notes

- **Do NOT** add extra fields beyond the three above. The manifest must remain minimal.
- Update `last_sync_commit` and `last_sync_timestamp` after every `init`, `ingest` (when operating on git commits), or `sync` operation.
- Retrieve the current git HEAD SHA with:

  ```bash
  git rev-parse HEAD
  ```
