# smooth_logger

A simple logger made primarily for my own personal use. Was made out of a combination of necessity and being so lazy that I overflowed into being productive and instead of searching for a library that suited my needs, I wrote my own.

## Installation

smooth-logger can be installed through pip. Either download the latest release from Codeberg/GitHub, or do `pip install smooth-logger` to install from PyPi. For the latest commits, check the `dev` branches on the repositories.

smooth-logger was written in Python 3.9, but should work with Python 3.5 and up. A minimum of 3.5 is required due to the project's use of type hinting, which was introduced in that version.

smooth-logger supports Linux, macOS and Windows.

## Usage

Usage of smooth-logger is, as it should be, quite simple.

The `Logger` model provides a number of methods for your use:

- `Logger.clean()` erases all log entries currently in memory.
- `Logger.get()` allows you to retrieve either the most recent log entry or all log entries, optionally filtered by scope.
- `Logger.get_time()` returns the full date & time, or optionally just the date, in ISO-8601 formatting.
- `Logger.init_bar()` initialises the `ProgressBar` model imported from the `smooth_progress` dependency.
- `Logger.notify()` sends a desktop notification using the `plyer` dependency.
- `Logger.new()` creates and, depending on scope, prints a new log entry.
- `Logger.output()` saves all log entries of appropriate scope to the log file and cleans the log array for the next group of log entries. A new log file is created for each new day. This method only attempts to create or update the log file if there are entries of an appropriate scope to be written to it; if there are none, it just executes `Logger.clean()`.

When initialising the Logger, you can optionally provide values to associate with each scope:

- 0: disabled, do not print to console or save to log file
- 1: enabled, print to console but do not save to log file
- 2: maximum, print to console and save to log file

The scopes available, along with their default values and suggested use cases, are:

- DEBUG (0): Information for debugging the program.
- ERROR (2): Errors that the program can recover from but impact functionality or performance.
- FATAL (2): Errors that mean the program must continue; handled crashes.
- INFO (1): General information for the user.
- WARNING (2): Things that have no immediate impact to functionality but could cause errors later on.

Here is a simple example showing the initialisation of the logger:

```py
import smooth_logger

Log = smooth_logger.Logger("Example", "~/.config/example")
Log.new("This is a log message!", "INFO")
```

## Roadmap

A roadmap of planned future improvements and features:

- Allow the creation of custom scopes. These would be instance-specific and not hard saved in any way. Suggested format and example:

  ```
  Log.add_scope(name: str, description: str, default_value: int)
  
  Log.add_scope("NEWSCOPE", "A new scope of mine!", 1)
  ```
  
  Potentially also allow removal of scopes. In this situation, default scopes should be removable, but doing so should log a warning.
  

- Allow editing of the values of existing scopes post-initialisation. For example:

  ```
  Log.edit_scope(name: str, new_value: int)
  
  Log.edit_scope("DEBUG", 1)
  ```
  
  to temporarily enable debug statements. This feature would probably see the most use from custom scopes.
  

- Add an optional argument `notify: bool` to `Logger.new()` to allow log entries to be created and notified in one statement, rather than the two currently required.

- Rework `Logger.get()` to allow passing of a specific number of log values to be fetched. If these values exceed the number in the log, all matching log values should be returned, and a warning should be issued (but not returned).

- Possibly replace some internal warnings with Exceptions so they can be more easily-handled by end-user programs.