# License Selection

Ask the user to choose a license when the repository does not already declare one. These are common, distinct choices, not a universal ranking and not legal advice:

| Choice | Concise tradeoff |
|---|---|
| `MIT` | Short permissive terms allowing broad reuse, modification, sublicensing, and sale when the copyright and permission notice are retained. It contains no explicit patent grant. |
| `Apache-2.0` | Permissive terms with an explicit contributor patent license, patent-litigation termination, changed-file notices, and NOTICE handling when applicable. |
| `GPL-3.0-only` | Strong copyleft: conveyed modified source works remain under GPL v3, and corresponding source obligations apply to conveyed object code. Use `GPL-3.0-or-later` only if the user explicitly wants the later-version option. |
| Another or custom choice | The renderer accepts only a valid SPDX expression, including `LicenseRef-...` for a custom license, not arbitrary legal text. Explain that custom legal text may warrant legal review. |

Never claim one option is universally best or most popular.

## Authoritative Sources

- MIT SPDX record and text: https://spdx.org/licenses/MIT.html
- Apache License 2.0 SPDX record and text: https://spdx.org/licenses/Apache-2.0.html
- Apache Software Foundation license text: https://www.apache.org/licenses/LICENSE-2.0
- GPL v3 only SPDX record and text: https://spdx.org/licenses/GPL-3.0-only.html
- GNU GPL v3 license text: https://www.gnu.org/licenses/gpl-3.0.html
