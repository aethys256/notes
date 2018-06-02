## macOS defaults

**Credits**
- https://github.com/mathiasbynens/dotfiles/blob/master/.macos
- https://github.com/joeyhoer/starter

It's possible that some of them needs to be done manually (there are some `TODO` remaining).
Once it'll be a bit more complete, I'll probably make an unattended script.

### General
```sh
# Appearance: "Blue" For Buttons, Menus, and Windows
defaults write NSGlobalDomain AppleAquaColorVariant -int 1

# "enable" Use dark menu bar and Dock
defaults write NSGlobalDomain AppleInterfaceStyle -string 'Dark'

# "disable" Automatically hide and show the menu bar
defaults write NSGlobalDomain _HIHideMenuBar -bool false

# Highlight color: "Blue"
defaults write NSGlobalDomain AppleHighlightColor -string '0.780400 0.815700 0.858800'

# Sidebar icon size: "Medium"
defaults write NSGlobalDomain NSTableViewDefaultSizeMode -int 2

# Show scroll bars: "Always"
defaults write NSGlobalDomain AppleShowScrollBars -string Always

# Click in the scroll bar to "Jump to the spot that's clicked"
defaults write NSGlobalDomain AppleScrollerPagingBehavior -int 1

# "disable" Ask to keep changes when closing documents
defaults write NSGlobalDomain NSCloseAlwaysConfirmsChanges -bool false

# "enable" Close windows when quitting an app
defaults write NSGlobalDomain NSQuitAlwaysKeepsWindows -bool false

# "enable" Use LCD font smoothing when available
# TODO

```

### Desktop & Screen Saver
```sh
# Start after: "Never"
defaults -currentHost write com.apple.screensaver idleTime -int 0

```

### Dock
```sh
# Icon "size"
defaults write com.apple.dock tilesize -int 45

# "enable" Magnification
defaults write com.apple.dock magnification -bool true

# Magnification "size"
defaults write com.apple.dock largesize -int 60

# Position on screen: "Bottom"
defaults write com.apple.dock orientation -string 'bottom'

# Minimize windows using: "Genie effect"
defaults write com.apple.dock mineffect -string 'genie'

# Prefer tabs when opening documents: "Manually"
defaults write NSGlobalDomain AppleWindowTabbingMode -string 'manual'

# "enable" Double-click a window's title bar to "minimize"
defaults write NSGlobalDomain AppleActionOnDoubleClick -string 'Minimize'

# "enable" Minimize windows into application icon
defaults write com.apple.dock minimize-to-application -bool true

# "enable" Animate opening applications
defaults write com.apple.dock launchanim -bool true

# "enable" Automatically hide and show the Dock
defaults write com.apple.dock autohide -bool true

# "enable" Show indicators for open applications
defaults write com.apple.dock show-process-indicators -bool true

# Remove the auto show/hide trigger delay
defaults write com.apple.dock autohide-delay -float 0

# Reduce the auto show/hide animation duration
defaults write com.apple.dock autohide-time-modifier -float 0.3

```

### Mission Control
```sh
# "disable" Automatically rearrange Spaces based on most recent use
defaults write com.apple.dock mru-spaces -bool false

# "enable" When switching to an application, switch to a Space with open windows for the application
defaults write NSGlobalDomain AppleSpacesSwitchOnActivate -bool true

# "disable" Group windows by application
defaults write com.apple.dock expose-group-by-app -bool false

# "enable" Displays have seperate Spaces
defaults write com.apple.spaces spans-displays -bool true

# Dashboard: "Off"
defaults write com.apple.dashboard dashboard-enabled-state -int 1

## Hot Corners...
# Top left: "Mission Control"
defaults write com.apple.dock wvous-tl-corner   -int 2
defaults write com.apple.dock wvous-tl-modifier -int 0
# Top right: "Launchpad"
defaults write com.apple.dock wvous-tr-corner   -int 11
defaults write com.apple.dock wvous-tr-modifier -int 0
# Bottom left: "Launchpad"
defaults write com.apple.dock wvous-bl-corner   -int 11
defaults write com.apple.dock wvous-bl-modifier -int 0
# Bottom right: "Desktop"
defaults write com.apple.dock wvous-br-corner   -int 4
defaults write com.apple.dock wvous-br-modifier -int 0
# Why two times "Launchpad" ? It's because of dual monitor setup, to have 3 hot corners on each
# it is needed to put them in an asymetric way (the right one being a little bit upper, with the
# left one being the principal one).

```

### Security & Privacy
```sh
# Disable the “Are you sure you want to open this application?” dialog
defaults write com.apple.LaunchServices LSQuarantine -bool false

```

### Display
```sh
# "enable" Show mirroring options in the menu bar when available
defaults write com.apple.airplay showInMenuBarIfPresent -bool true

# Subpixel font rendering on non-Apple LCDs
# 0 : Disabled | 1 : Minimal | 2 : Medium | 3 : Smoother | 4 : Strong
defaults write NSGlobalDomain AppleFontSmoothing -int 1

## Night Shift
# See: https://www.reddit.com/r/osx/comments/6334ac/toggling_night_shift_from_script/
# sudo defaults read com.apple.CoreBrightness

# Schedule: "Custom"
# TODO

# From: "22:00" to: "07:30"
# TODO

# Color Temperature: 2700 (More Warm)
# TODO

```

### Energy Saver
```sh
# Turn display off after: "300" (5 minutes)
# TODO

# "enable" Prevent computer from sleeping automatically when the display is off
# TODO

# "disable" Put hard disks to sleep when possible
# TODO

# "disable" Wake for Ethernet network access
# TODO

# "disable" Start up automatically after a power failure
# TODO

# "disable" Enable Power Nap
# TODO

```

### Keyboard
```sh
# Key Repeat "speed"
defaults write NSGlobalDomain KeyRepeat -int 3

# "delay" Until Repeat
defaults write NSGlobalDomain InitialKeyRepeat -int 10

# "enable" Adjust keyboard brightness in low light
# TODO

# "enable" Turn keyboard backlight off after "10 secs" of inactivity
# TODO

# "disable" Show keyboard and emoji viewers in menu bar
# TODO

# "disable" Use F1, F2, etc. keys as standard function keys
defaults write NSGlobalDomain com.apple.keyboard.fnState -bool false

# "disable" Correct spelling automatically
defaults write NSGlobalDomain NSAutomaticSpellingCorrectionEnabled -bool false

# "disable" Capitalize words automatically
defaults write NSGlobalDomain NSAutomaticCapitalizationEnabled -bool false

# "disable" Add period with double-space
defaults write NSGlobalDomain NSAutomaticPeriodSubstitutionEnabled -bool false

# "disable" Use smart quotes and dashes
defaults write NSGlobalDomain NSAutomaticDashSubstitutionEnabled -bool false
defaults write NSGlobalDomain NSAutomaticQuoteSubstitutionEnabled -bool false

# Shortcuts
# Launchpad & Dock:
  # Uncheck: "Turn Dock Hiding On/Off"
  # Check: "Show Launchpad" (CTRL + DOWN arrow)
# Display:
  # Uncheck: Everything
# Mission Control:
  # Uncheck: "Application windows", "Show Dashboard"
  # Check: "Show Notification Center" (F12)
# Keyboard:
  # Uncheck: Everything

# Full Keyboard Access: In windows and dialogs, press Tab to move keyboard focus between:
# "All controls"
defaults write NSGlobalDomain AppleKeyboardUIMode -int 2

# "enable" Show input menu in menu bar
# TODO

# Disable Press-And-Hold for keys
defaults write NSGlobalDomain ApplePressAndHoldEnabled -bool false

```

### Mouse
```sh
# "enable" Scroll direction: Natural
# TODO

# Primary mouse buton: "Left"
# TODO
```

### Trackpad
```sh
# TODO

# "enable" Scroll direction: Natural
# TODO

```

### Sound
```sh
# "enable" Play user interface sound effects
defaults write com.apple.systemsound com.apple.sound.uiaudio.enabled -bool true

# "disable" Play feedback when volume is changed
defaults write NSGlobalDomain com.apple.sound.beep.feedback -bool false

# Show volume in the menu bar
# TODO Fix
defaults write com.apple.systemuiserver "NSStatusItem Visible com.apple.menuextra.volume" -int 0

```

### iCloud
```sh
# Save to disk (not to iCloud) by default
defaults write NSGlobalDomain NSDocumentSaveNewDocumentsToCloud -bool false

```

### App Store
```sh
# Do not complain about admin password for the next 5 minutes
sudo -v

```
```sh
# "enable" Automatically check for updates
sudo defaults write /Library/Preferences/com.apple.SoftwareUpdate AutomaticCheckEnabled -bool true

# "enable" Download newly available updates in the background
sudo defaults write /Library/Preferences/com.apple.SoftwareUpdate AutomaticDownload -bool true

# "disable" Install app updates
sudo defaults write /Library/Preferences/com.apple.commerce AutoUpdate -bool true

# "disable" Install macOS updates
sudo defaults write /Library/Preferences/com.apple.commerce AutoUpdateRestartRequired -bool true

# "disable" Install system ddata files and security updates
sudo defaults write /Library/Preferences/com.apple.SoftwareUpdate ConfigDataInstall -bool true
sudo defaults write /Library/Preferences/com.apple.SoftwareUpdate CriticalUpdateInstall -bool true

# Check for software updates daily, not just once per week
defaults write com.apple.SoftwareUpdate ScheduleFrequency -int 1

```

### Siri
```sh
# "disable" Enable Ask Siri
defaults write com.apple.assistant.support "Assistant Enabled" -bool false

# "disable" Show Siri in menu bar
defaults write com.apple.Siri StatusMenuVisible -bool false

```

### Accessibility
```sh
# "disable" Enable Slow Keys
defaults write com.apple.universalaccess slowKey -bool false

```

### Finder
```sh
# Show these items on the desktop: "Hard disks", "External disks", "CDs, DVDs, and iPods", "Connected servers"
defaults write com.apple.finder ShowHardDrivesOnDesktop -bool true
defaults write com.apple.finder ShowExternalHardDrivesOnDesktop -bool true
defaults write com.apple.finder ShowRemovableMediaOnDesktop -bool true
defaults write com.apple.finder ShowMountedServersOnDesktop -bool true

# New Finder windows show: "Home folder"
defaults write com.apple.finder NewWindowTarget -string 'PfHm'
defaults write com.apple.finder NewWindowTargetPath -string "file://${HOME}/"

# "disable" Open folders in tabs instead of new windows
defaults write com.apple.finder FinderSpawnTab -bool false

# "enable" only:
# Recents
# Applications
# Desktop
# Documents
# Downloads
# Home
#
# Back to My Mac
# Connected servers
# Bonjour computers
#
# This PC
# Hard disks
# External disks
# CDs, DVDs, and iPods
# "disable" Sidebar: Recent Tags
defaults write com.apple.finder ShowRecentTags -bool false

# "enable" Show all filename extensions
defaults write NSGlobalDomain AppleShowAllExtensions -bool true

# "disable" Show warning before changing an extension
defaults write com.apple.finder FXEnableExtensionsChangeWarning -bool false

# "disable" Show warning before removing from iCloud Drive
defaults write com.apple.finder FXEnableRemoveFromICloudDriveWarning -bool false

# "disable" Show warning before emptying the Trash
defaults write com.apple.finder WarnOnEmptyTrash -bool false

# "disable" Remove items from the Trash after 30 days
defaults write com.apple.finder FXRemoveOldTrashItems -bool false

# "enable" Keep folders on top when sorting by name
defaults write com.apple.finder _FXSortFoldersFirst -bool true

# When performing a search: "Search the Current Folder"
defaults write com.apple.finder FXDefaultSearchScope -string 'SCcf'

# "enable" Path Bar
defaults write com.apple.finder ShowPathbar -bool true

# "enable" Status Bar
defaults write com.apple.finder ShowStatusBar -bool true

# Show hidden files by default
defaults write com.apple.finder AppleShowAllFiles -bool true

# Default view style (List View)
defaults write com.apple.Finder FXPreferredViewStyle -string 'Nlsv'

# "disable" Writing of .DS_Store files on network or USB volumes
defaults write com.apple.desktopservices DSDontWriteNetworkStores -bool true
defaults write com.apple.desktopservices DSDontWriteUSBStores -bool true

# Show absolute path in Finder's title bar.
defaults write com.apple.finder _FXShowPosixPathInTitle -bool true

# Expand save panel by default.
defaults write NSGlobalDomain NSNavPanelExpandedStateForSaveMode -bool true
defaults write NSGlobalDomain NSNavPanelExpandedStateForSaveMode2 -bool true

# Expand print panel by default.
defaults write NSGlobalDomain PMPrintingExpandedStateForPrint -bool true
defaults write NSGlobalDomain PMPrintingExpandedStateForPrint2 -bool true

# Automatically quit printer app once the print jobs complete
defaults write com.apple.print.PrintingPrefs "Quit When Finished" -bool true

```

### Safari
```sh
# Safari opens with: "All windows from last sessions"
defaults write com.apple.Safari AlwaysRestoreSessionAtLaunch -bool true

# New windows open with: "Homepage"
defaults write com.apple.Safari NewWindowBehavior -int 0

# New tabs open with: "Homepage"
defaults write com.apple.Safari NewTabBehavior -int 0

# Set home page to "https://www.google.com/"
# Not easy, do it manually

# Remove history items: "Manually"
defaults write com.apple.Safari HistoryAgeInDaysLimit -int 365000

# Favorites shows: "Favorites"
# Not easy, do it manually

# Top Sites shows: "12 sites"
defaults write com.apple.Safari TopSitesGridArrangement -int 1

# File download location: "Downloads"
# Not easy, do it manually

# Remove downloads list items: "Manually"
defaults write com.apple.Safari DownloadsClearingPolicy -int 0

# "disable" Open "safe" files after downloading
defaults write com.apple.Safari AutoOpenSafeDownloads -bool false

# Open pages in tabs instead of windows: "Automatically"
defaults write com.apple.Safari TabCreationPolicy -int 1

# "enable" Command-click opens a link in a new tab
# TODO

# "disable" When a new tab or window opens, make it active
# TODO

# "enable" Use command-1 through command-9 to switch tabs
# TODO

# AutoFill web forms: None
defaults write com.apple.Safari AutoFillFromAddressBook -bool false
defaults write com.apple.Safari AutoFillPasswords -bool false
defaults write com.apple.Safari AutoFillCreditCardData -bool false
defaults write com.apple.Safari AutoFillMiscellaneousForms -bool false

# Search engine: "Google"
# Not easy, do it manually

# "disable" Include search engine suggestions
defaults write com.apple.Safari SuppressSearchSuggestions -bool true

# "disable" Include Safari Suggestions
defaults write com.apple.Safari UniversalSearchEnabled -bool false

# "disable" Enable Quick Website Search
defaults write com.apple.Safari WebsiteSpecificSearchEnabled -bool false

# "disable" Preload Top Hit in the background
defaults write com.apple.Safari PreloadTopHit -bool false

# "enable" Show Favorites
# Not easy, do it manually

# "disable" Warn when visiting a fraudulent website
defaults write com.apple.Safari WarnAboutFraudulentWebsites -bool false

# "enable" Enable JavaScript
defaults write com.apple.Safari WebKitJavaScriptEnabled -bool true
defaults write com.apple.Safari com.apple.Safari.ContentPageGroupIdentifier.WebKit2JavaScriptEnabled -bool true

# "enable" Block pop-up windows
defaults write com.apple.Safari WebKitJavaScriptCanOpenWindowsAutomatically -bool false
defaults write com.apple.Safari com.apple.Safari.ContentPageGroupIdentifier.WebKit2JavaScriptCanOpenWindowsAutomatically -bool false

# "enable" Prevent cross-site tracking && "disable" Block all cookies
defaults write com.apple.Safari BlockStoragePolicy -int 2
defaults write com.apple.Safari WebKitStorageBlockingPolicy -int 1
defaults write com.apple.Safari com.apple.Safari.ContentPageGroupIdentifier.WebKit2StorageBlockingPolicy -int 1

# "enable" Ask websites not to track me
defaults write com.apple.Safari SendDoNotTrackHTTPHeader -bool true

# "enable" Show full website address
defaults write com.apple.Safari ShowFullURLInSmartSearchField -bool true

# "disable" Press Tab to highlight each item on a webpage
defaults write com.apple.Safari WebKitTabToLinksPreferenceKey -bool false

# "disable" Save articles for offline reading automatically
defaults write com.apple.Safari ReadingListSaveArticlesOfflineAutomatically -bool false

# "enable" Stop plug-ins to save power
defaults write com.apple.Safari com.apple.Safari.ContentPageGroupIdentifier.WebKit2PlugInSnapshottingEnabled -bool true

# Default encoding: "Unicode (UTF-8)"
defaults write com.apple.Safari WebKitDefaultTextEncodingName -string 'utf-8'
defaults write com.apple.Safari com.apple.Safari.ContentPageGroupIdentifier.WebKit2DefaultTextEncodingName -string 'utf-8'

# "enable" Show Develop menu in menu bar
defaults write com.apple.Safari IncludeDevelopMenu -bool true
defaults write com.apple.Safari WebKitDeveloperExtrasEnabledPreferenceKey -bool true
defaults write com.apple.Safari com.apple.Safari.ContentPageGroupIdentifier.WebKit2DeveloperExtrasEnabled -bool true

```

### Extras
```sh
# Terminal: "Pro" theme as default
defaults write com.apple.terminal "Default Window Settings" -string 'Pro'
defaults write com.apple.terminal "Startup Window Settings" -string 'Pro'

# Terminal: "Pro" theme Window Size: Columns = 155 | Rows = 35
# TODO

# Terminal: Only use UTF-8
defaults write com.apple.terminal StringEncodings -array 4

# iTunes: Don't automatically sync connected devices
defaults write com.apple.itunes dontAutomaticallySyncIPods -bool true

# Disk Utility: Show All Devices
defaults write com.apple.DiskUtility SidebarShowAllDevices -bool true

# Time Machine: Prevent from prompting to use new hard drives as backup volume
defaults write com.apple.TimeMachine DoNotOfferNewDisksForBackup -bool true

```

### Misc
Should be applied manually -one by one- if needed
```sh
# Increase mouse speed beyond the max (GUI max is 3.0), useful mostly for Magic Mouse
defaults write -g com.apple.mouse.scaling -float 5.0

# Disable automatic termination of inactive apps
defaults write NSGlobalDomain NSDisableAutomaticTermination -bool true

```
Do not forget to manually review **Security & Privacy**, **Displays**, **Inputs (Keyboard/Mouse/Trackpad)**, **Sharing** and **Users & Groups** then `reboot`.
