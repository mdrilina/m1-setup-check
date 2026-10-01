# Ko dara `make test`

`make test` palaiž `Makefile` mērķi `test`, kas izpilda vienu komandu:

```make
test:
	pytest -q
```

## Kas notiek

1. **`pytest` meklē testus** visā repozitorijā, sākot no saknes mapes. Tas atrod failus `test_*.py` un tajos funkcijas `test_*`. Šobrīd vienīgais testu fails ir `tests/test_smoke.py`.
2. **`-q`** (quiet) saīsina izvadi: katrs izturēts tests ir viens punkts, un beigās ir kopsavilkums.
3. **Tiek izpildīti divi smoke testi:**
   - `test_openapi_document_can_be_loaded` ielādē `docs/openapi.yaml` ar `yaml.safe_load`. Tas pārbauda, ka lauks `openapi` sākas ar `3.` un ka sadaļa `paths` nav tukša. Tātad līgums ir korekts YAML ar OpenAPI 3.x pamatstruktūru.
   - `test_participant_files_are_present` pārbauda, ka eksistē obligātie faili: `.claude/settings.json`, `.devcontainer/devcontainer.json`, `CLAUDE.md`, `Makefile`, `tracker/CR-2.md` un `tracker/README.md`. Ja kāda trūkst, kļūdas ziņojumā tie ir uzskaitīti.
4. **Izejas kods:** ja visi testi iziet, komanda beidzas ar `0`. Ja kāds neiziet, tā beidzas ar kodu, kas nav nulle, un `make` ziņo par kļūdu. Pēc šī koda arī CI nosaka, vai pārbaude ir zaļa vai sarkana.

## Rezultāts

```
..                                                                       [100%]
2 passed in 0.09s
```

## Ko `make test` nepārbauda

- **API līguma kursa noteikumus.** `400`, `404`, `5xx` atbildes un kopīgo `Error` shēmu pārbauda `make lint-contract`, nevis `make test`.
- **Vidi.** `make verify-setup` papildus pārbauda Python, pakotnes, Claude Code un failu `setup/claude-answer.md`. Tas palaiž tikai `tests/test_smoke.py`, savukārt `make test` palaiž visus testus, arī tos, kas tiks pievienoti vēlāk.
