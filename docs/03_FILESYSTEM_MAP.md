# Switchboard Filesystem Map

The original source documents identify the main platform root as:

```text
/mnt/eila-hot-sidecar/switchboard-platform
```

## Ecosystem Layout

```text
switchboard-platform
├── 00-admin
├── 01-config
├── 02-databases
├── 03-core
├── 04-modules
├── 05-plugins
├── 06-apps
├── 07-retrieval
├── 08-client-data
├── 09-build-runs
├── 10-deploy
├── 11-docs
├── 12-tests
├── 13-resource-packs
├── 14-factory-assets
├── assets
├── data-volumes
└── product-exports
```

## CRM Application

```text
06-apps
└── web-crm
    ├── frontend
    │   └── blog-refine-supabase-auth
    ├── backend
    └── storage
```

## Frontend Module Pattern

```text
src/modules
├── automation360
├── billing360
├── comms360
├── contacts360
├── deals360
├── documents360
├── marketing360
├── reporting360
├── settings360
├── storage360
├── treasury360
├── wallet
└── workspace360
```

## Storage360 Example

```text
src/modules/storage360
├── components
├── data
├── lib
├── page.tsx
├── sidecars
├── tabs
└── README.md
```

## Treasury360 Example

```text
src/modules/treasury360
├── api
├── cards
├── components
├── data
├── filters
├── graph
├── hooks
├── lib
├── page.tsx
├── primitives
├── sidecars
├── tabs
└── types
```

## Retrieval / Runtime Layer

The architecture documents identify retrieval and resource-pack systems used internally for code discovery, module assembly, and context generation.

These are intentionally excluded from the public repository but may be reviewed privately upon request.
