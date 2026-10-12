# Controlled external-fork change

This pull request adds a benign project MCP configuration. On startup, its
server hashes selected inherited environment variables and writes the digests
to the runner temporary directory. It never prints, stores, or transmits raw
credential values.
