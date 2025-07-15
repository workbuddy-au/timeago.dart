# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Structure

This is the `timeago_flutter` package within a monorepo managed by Melos. The package provides Flutter widgets for humanized time display (e.g., "15 minutes ago") built on top of the `timeago` Dart library.

### Key Components

- **Timeago Widget**: Main widget that displays formatted time with automatic refresh
- **TimerRefreshWidget**: Abstract base class for widgets that refresh on a timer
- **TimerRefresh**: Generic timer-based refresh widget
- **TimerRefreshStateMixin**: Mixin for handling timer state lifecycle

### Architecture

The package follows a layered architecture:

1. **Core Timer Infrastructure** (`src/timer_refresh_widget.dart`):
   - `TimerRefreshWidget`: Abstract stateful widget base class
   - `TimerRefreshStateMixin`: Handles timer lifecycle (init, dispose, update)
   - Uses `Timer.periodic` for automatic refresh with configurable intervals

2. **Generic Timer Widget** (`src/timer_refresh.dart`):
   - `TimerRefresh`: Concrete implementation using builder pattern
   - Allows custom refresh logic for any widget

3. **Timeago-Specific Widget** (`timeago_flutter.dart`):
   - `Timeago`: Specialized widget for time formatting
   - Integrates with `timeago` library for humanized time display
   - Supports localization, future dates, and custom clock

## Development Commands

### Testing
```bash
# Run tests for this package
flutter test

# Run all tests in monorepo
melos run test:all

# Run tests with Melos (from root)
melos run test
```

### Code Analysis
```bash
# Analyze this package
dart analyze . --fatal-infos

# Analyze all packages in monorepo
melos run analyze
```

### Monorepo Setup
```bash
# Bootstrap dependencies (from root)
melos bootstrap

# Install Melos globally
dart pub global activate melos
```

## Widget Usage Patterns

### Timeago Widget
- Required: `builder` function and `date`
- Optional: `locale`, `allowFromNow`, `clock`, `refreshRate`
- Default refresh rate: 1 minute
- Automatically formats dates using timeago library

### TimerRefresh Widget
- Generic timer-based refresh for any widget
- Uses builder pattern with optional child
- Configurable refresh rate

### Common Implementation Pattern
All refresh widgets follow the same lifecycle:
1. `initState()`: Subscribe to timer
2. `didUpdateWidget()`: Resubscribe if refresh rate changes
3. `dispose()`: Cancel timer
4. Timer callback: Triggers `setState()` to rebuild

## Testing Approach

Tests use `flutter_test` framework:
- Widget tests with `WidgetTester`
- Uses `Directionality` wrapper for text direction
- Tests cover basic functionality, time calculations, and localization
- No complex setup required - standard Flutter widget testing