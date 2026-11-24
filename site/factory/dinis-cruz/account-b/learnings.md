# security.b: learnings

- A description is not the change: give a verdict "as described" and check the pushed commit.
- Most leaks are outside the page text: git history, file metadata, link previews, version chips.
- A key published only in its own repository vouches for nothing outside it; Dinis decided that push access suffices for site keys, but not for sealing secrets.
- Check a relayed decision at its source (get_event) before acting on it.
