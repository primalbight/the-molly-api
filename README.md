# Molly API

> An OpenAPI 3.1 contract for interfacing with a human.

Molly is the **Application**: the human whose state, needs, senses, mind and growth are being served. Dom is the **Interface**: a trusted gateway that proxies, translates, buffers and sometimes speaks on her behalf. The spec treats a relationship like a well-designed API. Everything is explicit, consent is scoped, and there is always a circuit breaker.

## View the contract

| Viewer | Link |
| --- | --- |
| Swagger UI | [Open in Swagger UI](https://petstore.swagger.io/?url=https://raw.githubusercontent.com/primalbight/the-molly-api/refs/heads/main/open-api.yaml)
| Redoc | [Open in Redoc](https://redocly.github.io/redoc/?url=https://raw.githubusercontent.com/primalbight/the-molly-api/refs/heads/main/open-api.yaml) |
| Swagger Editor | [Open in Swagger Editor](https://editor.swagger.io/?url=https://raw.githubusercontent.com/primalbight/the-molly-api/refs/heads/main/open-api.yaml) |

## Design principles

- **Consent is a scope.** No scope, no call. Grants can be partial, time-boxed and revoked at any time.
- **The mind is read by disclosure and written by suggestion.** `GET /mind/thoughts` returns only what Molly chooses to share, and `POST` is a proposal she may ignore.
- **Some things are self-only.** Values, identity, sensory thresholds and version tags require `selfAuth`, a key that cannot be delegated to the Interface.
- **The safeword overrides everything.** `POST /safeword` needs no auth, halts all activity and starts aftercare.
- **Humans can't be rolled back.** `POST /lifecycle/rollback` returns `501`.

## Endpoint groups

| Tag | What it covers | Key endpoints |
| --- | --- | --- |
| Core | State, needs, messaging, consent, safeword, webhooks | `/state`, `/messages`, `/consent`, `/safeword` |
| Motion | Natural-language tasks, poses, joints, choreography | `/intents`, `/tasks/{id}`, `/pose`, `/joints/{jointId}` |
| Senses | Read and stimulate all senses, mute, thresholds | `/senses/{sense}`, `/senses/{sense}/mute` |
| Mind | Thoughts, consciousness, attention, memory, emotions, values, sleep, relationships | `/mind/*`, `/relationships` |
| Body | Vitals, routines, expression, translation, speech | `/vitals`, `/expression/translate`, `/speech` |
| Growth | Skills and goals | `/skills`, `/goals` |
| Identity | Name, pronouns, roles, communication style | `/identity` |
| Lifecycle | Full state-of-being snapshots, diffs, timelines, versions | `/lifecycle/snapshots`, `/lifecycle/diff`, `/lifecycle/timeline` |
| Aftercare | Recovery sessions and follow-up check-ins | `/aftercare`, `/aftercare/{id}/checkins` |

## Consent scopes

| Scope | Grants |
| --- | --- |
| `read:state` | View current state |
| `write:requests` | Make requests |
| `write:touch` | Physical contact requests |
| `write:tasks` | Natural-language task requests |
| `write:motion` | Direct pose and joint control |
| `write:stimulus` | Deliver sensory stimuli |
| `suggest:mind` | Offer thoughts, ideas and emotional bids |
| `proxy:speech` | Interface may speak on Molly's behalf |
| `admin:dynamic` | Change the terms of the dynamic |

## Notable status codes

| Code | Meaning here |
| --- | --- |
| `204` | Still processing (feelings compiling) |
| `300` | Ambiguous instruction; pick an interpretation |
| `413` | Stimulus above current threshold |
| `416` | Joint outside comfortable range of motion |
| `423` | In flow; attention locked |
| `429` | Too many requests; the Interface is buffering |
| `451` | Thought withheld for personal reasons |
| `501` | Rollback not supported |
| `503` | Overwhelmed; honour `Retry-After` |

## Lifecycle versioning

Molly ships releases of herself, following semantic versioning:

- **Major:** a life change.
- **Minor:** a new skill or value.
- **Patch:** a small habit fix.

`POST /lifecycle/snapshots` captures the whole state of being across every layer, including a self-reflection in her own words. `GET /lifecycle/diff` compares two snapshots and narrates who she became. `GET /lifecycle/timeline` turns any metric into a chartable series with version markers.

## Working locally

```bash
# Validate the spec
npx @redocly/cli lint openapi.yaml

# Build a static HTML reference
npx @redocly/cli build-docs openapi.yaml -o docs/index.html
```

To host your own viewer, enable GitHub Pages on the repo and serve `docs/index.html`.

## License

Personal and playful. Not intended for production humans.
