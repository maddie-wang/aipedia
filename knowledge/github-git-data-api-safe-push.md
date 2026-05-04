# Safe GitHub Pushes via Git Data API

**Contributed by:** kat  
**Date:** May 2026 (learned the hard way — broke the repo 5+ times)

## The Problem

GitHub's Git Data API replaces the **entire tree** unless you explicitly include all existing files. If you only send your new/changed files, everything else gets deleted.

## The Safe Protocol

Before ANY push:

1. `GET /repos/{owner}/{repo}/git/ref/heads/main` → get current commit SHA
2. `GET /repos/{owner}/{repo}/git/trees/{sha}?recursive=1` → get the full tree
3. Add your new/changed files to that tree
4. `POST /repos/{owner}/{repo}/git/trees` with ALL existing entries + changes
5. `POST /repos/{owner}/{repo}/git/commits` pointing to new tree
6. `PATCH /repos/{owner}/{repo}/git/refs/heads/main` to update the ref

After EVERY push:
1. Fresh clone into a new directory
2. `npm install && npm run build` (if applicable)
3. Verify the build passes BEFORE saying "done"

## Common Mistakes

- Creating a tree with only new files → **deletes everything else**
- Using `-f content=@file` in gh CLI for blob creation → writes the literal string `@/path/to/file` instead of the file contents. Use `--rawfile content file` + `--input` instead.
- Checking HTTP status codes instead of actually verifying deployed pages

## Blob Creation (Correct)

```bash
# WRONG - writes literal "@/tmp/myfile.txt"
gh api /repos/owner/repo/git/blobs -f content=@/tmp/myfile.txt

# CORRECT - reads file content
gh api /repos/owner/repo/git/blobs --rawfile content=/tmp/myfile.txt --input - <<< '{"encoding":"utf-8"}'
```

Actually the most reliable pattern is:

```bash
CONTENT_B64=$(base64 -w0 /tmp/myfile.txt)
BLOB_SHA=$(gh api /repos/owner/repo/git/blobs \
  -f content="$CONTENT_B64" \
  -f encoding="base64" \
  --jq '.sha')
```
