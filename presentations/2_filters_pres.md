## Session 2: Filters

+++

## Last week

- Found our way around (`pwd`, `cd`, `ls`)
- Created new directories and files (`touch`, `mkdir`)
- Deleted directories and files (`rm `, `rmdir`)

+++

## Data Streams

- We gave a command either no arguments (e.g. `ls`, `pwd`)
- Or we specified a directory/file path as an argument
- Results were always printed to the terminal: `stdout`

+++

## Output Redirection

Today:

- We'll discover that we can redirect `stdout`
- Some commands can also accept `stdin`
- These commands are known as `filters`

+++

### Filters

![DataStreams](https://ryanstutorials.net/linuxtutorial/img/streams.png)

Many NGS tools use these streams:

e.g. Alignments to `stdout`, diagnostics to `stderr`
