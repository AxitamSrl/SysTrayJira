# TODO — SysTrayJira Roadmap

## Notifications
- [ ] Desktop notification when new issues appear in a group
- [ ] Badge/counter on tray icon with total issue count

## Menu
- [ ] Submenus per group (instead of flat list)
- [ ] "Open Jira Board" quick link
- [ ] Show time since last refresh in menu
- [ ] `max_results` per group config
- [ ] `sort_by` per group (priority, date, status)

## Actions
- [ ] Quick status transition from menu (e.g. "In Progress" → "In Review")
- [ ] Copy issue link to clipboard (right-click)
- [ ] Quick search popup by issue key

## Icon
- [ ] Custom icon per group
- [ ] Badge overlay with issue count on tray icon

## Config
- [ ] Config validation on load (warn on missing/invalid fields)
- [ ] Hot-reload config on file change (inotify/watchdog)

## Packaging
- [ ] Publish to PyPI
- [ ] Flatpak / Snap package for Linux
- [ ] Windows installer (.msi / .exe)
- [ ] macOS .app bundle with py2app
