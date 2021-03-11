# git show

## name

```text
git-show - Show various types of objects
```

## SYNOPSIS

```text
git show [<options>] [<object>…]
```

## DESCRIPTION

```
Shows one or more objects (blobs, trees, tags and commits).
For commits it shows the log message and textual diff. It also presents the merge commit in a special format as produced by *git diff-tree --cc*.
For tags, it shows the tag message and the referenced objects.
For trees, it shows the names (equivalent to *git ls-tree* with --name-only).
For plain blobs, it shows the plain contents.
The command takes options applicable to the *git diff-tree* command to control how the changes the commit introduces are shown.
```

```
显示一个或多个对象（斑点，树，标签和提交）。
对于提交，它将显示日志消息和文本差异。
它还以git diff-tree --cc产生的特殊格式显示合并提交。
对于标签，它显示标签消息和引用的对象。对于树，它显示名称（相当于git ls-tree和--name-only）。
对于普通斑点，它显示普通内容。该命令采用适用于git diff-tree命令的选项，以控制如何显示提交引入的更改。
```

## OPTIONS

<object>…

The names of objects to show (defaults to *HEAD*). For a more complete list of ways to spell object names, see "SPECIFYING REVISIONS" section in [gitrevisions(7)](gitrevisions.html).

--pretty[=<format>]

--format=<format>

Pretty-print the contents of the commit logs in a given format, where *<format>* can be one of *oneline*, *short*, *medium*, *full*, *fuller*, *reference*, *email*, *raw*, *format:<string>* and *tformat:<string>*. When *<format>* is none of the above, and has *%placeholder* in it, it acts as if *--pretty=tformat:<format>* were given.

See the "PRETTY FORMATS" section for some additional details for each format. When *=<format>* part is omitted, it defaults to *medium*.

Note: you can specify the default pretty format in the repository configuration (see [git-config(1)](git-config.html)).

--abbrev-commit

Instead of showing the full 40-byte hexadecimal commit object name, show a prefix that names the object uniquely. "--abbrev=<n>" (which also modifies diff output, if it is displayed) option can be used to specify the minimum length of the prefix.

This should make "--pretty=oneline" a whole lot more readable for people using 80-column terminals.

--no-abbrev-commit

Show the full 40-byte hexadecimal commit object name. This negates `--abbrev-commit`, either explicit or implied by other options such as "--oneline". It also overrides the `log.abbrevCommit` variable.

--oneline

This is a shorthand for "--pretty=oneline --abbrev-commit" used together.

--encoding=<encoding>

The commit objects record the encoding used for the log message in their encoding header; this option can be used to tell the command to re-code the commit log message in the encoding preferred by the user. For non plumbing commands this defaults to UTF-8. Note that if an object claims to be encoded in `X` and we are outputting in `X`, we will output the object verbatim; this means that invalid sequences in the original commit may be copied to the output.

--expand-tabs=<n>

--expand-tabs

--no-expand-tabs

Perform a tab expansion (replace each tab with enough spaces to fill to the next display column that is multiple of *<n>*) in the log message before showing it in the output. `--expand-tabs` is a short-hand for `--expand-tabs=8`, and `--no-expand-tabs` is a short-hand for `--expand-tabs=0`, which disables tab expansion.

By default, tabs are expanded in pretty formats that indent the log message by 4 spaces (i.e. *medium*, which is the default, *full*, and *fuller*).

--notes[=<ref>]

Show the notes (see [git-notes(1)](git-notes.html)) that annotate the commit, when showing the commit log message. This is the default for `git log`, `git show` and `git whatchanged` commands when there is no `--pretty`, `--format`, or `--oneline` option given on the command line.

By default, the notes shown are from the notes refs listed in the `core.notesRef` and `notes.displayRef` variables (or corresponding environment overrides). See [git-config(1)](git-config.html) for more details.

With an optional *<ref>* argument, use the ref to find the notes to display. The ref can specify the full refname when it begins with `refs/notes/`; when it begins with `notes/`, `refs/` and otherwise `refs/notes/` is prefixed to form a full name of the ref.

Multiple --notes options can be combined to control which notes are being displayed. Examples: "--notes=foo" will show only notes from "refs/notes/foo"; "--notes=foo --notes" will show both notes from "refs/notes/foo" and from the default notes ref(s).

--no-notes

Do not show notes. This negates the above `--notes` option, by resetting the list of notes refs from which notes are shown. Options are parsed in the order given on the command line, so e.g. "--notes --notes=foo --no-notes --notes=bar" will only show notes from "refs/notes/bar".

--show-notes[=<ref>]

--[no-]standard-notes

These options are deprecated. Use the above --notes/--no-notes options instead.

--show-signature

Check the validity of a signed commit object by passing the signature to `gpg --verify` and show the output.

```
<object> ......
要显示的对象名称（默认为HEAD）。
有关拼写对象名称的方法的完整列表，请参见gitrevisions（7）中的“指定版本”部分。 

--pretty [= <format>] 

--format = <format>
以给定格式漂亮地打印提交日志的内容，其中<format>可以是单行，短，中，完整，完整，引用，电子邮件，原始，格式：<string>和tformat：<string>。如果<format>都不是上述内容，并且其中包含％placeholder，则它的作用就像给出了--pretty = tformat：<format>一样。有关每种格式的一些其他详细信息，请参见“ PRETTY FORMATS”部分。当省略= <format>部分时，默认为中。注意：您可以在存储库配置中指定默认的漂亮格式（请参阅git-config（1））。 

--abbrev-commit而不是显示完整的40字节十六进制提交对象名称，而是显示一个以唯一方式命名对象的前缀。 “ --abbrev = <n>”（如果显示，它也会修改diff输出）选项可用于指定前缀的最小长度。对于使用80列终端的人来说，这应该使“ --pretty = oneline”更具可读性。 

--no-abbrev-commit
显示完整的40字节十六进制提交对象名称。这会否定--abbrev-commit，无论是显式表示还是由其他选项（例如“ --oneline”）暗含。它还将覆盖log.abbrevCommit变量。 

--oneline
这是一起使用的“ --pretty = oneline --abbrev-commit”的简写。 

--encoding = <encoding>
提交对象在其编码头中记录用于日志消息的编码；此选项可用于告诉命令以用户首选的编码方式重新编码提交日志消息。对于非管道命令，默认为UTF-8。注意，如果一个对象声称要用X编码并且我们要用X输出，我们将逐字输出该对象；这意味着可以将原始提交中的无效序列复制到输出中。 

--expand-tabs = <n> 
--expand-tabs 
--no-expand-tabs
在日志中执行选项卡扩展（用足够的空间替换每个选项卡以填充到下一个显示的列，该列是<n>的倍数）消息，然后在输出中显示它。 --expand-tabs是--expand-tabs = 8的简写，而--no-expand-tabs是--expand-tabs = 0的简写，它禁用了选项卡扩展。默认情况下，制表符以漂亮的格式展开，该格式将日志消息缩进4个空格（即中号，即默认，完整和完整）。 

--notes [= <ref>]
在显示提交日志消息时，显示注释提交的注释（请参阅git-notes（1））。当命令行上没有--pretty，--format或--oneline选项时，这是git log，git show和git whatchanged命令的默认设置。默认情况下，显示的注释来自core.notesRef和notes.displayRef变量（或相应的环境替代）中列出的注释refs。有关更多详细信息，请参见git-config（1）。通过可选的<ref>参数，使用ref查找要显示的注释。当ref以refs / notes /开头时，可以指定完整的refname；当以notes /，refs /开头，否则以refs / notes /开头，以形成ref的全名。可以组合多个--notes选项来控制显示哪些笔记。示例：“ --notes = foo”将仅显示“ refs / notes / foo”中的注释； “ --notes = foo --notes”将同时显示“ refs / notes / foo”中的注释和默认注释ref中的注释。 

--no-notes
不显示注释。通过重置显示注释的注释引用列表，可以取消上述--notes选项。选项是按照命令行中给出的顺序进行解析的，例如“ --notes --notes = foo --no-notes --notes = bar”将仅显示“ refs / notes / bar”中的注释。 

--show-notes [= <ref>]
--[no-] standard-notes
这些选项已弃用。请改用上述--notes /-no-notes选项。 

--show-signature
通过将签名传递给gpg --verify来检查签名的提交对象的有效性，并显示输出
```

## PRETTY FORMATS

If the commit is a merge, and if the pretty-format is not *oneline*, *email* or *raw*, an additional line is inserted before the *Author:* line. This line begins with "Merge: " and the hashes of ancestral commits are printed, separated by spaces. Note that the listed commits may not necessarily be the list of the **direct** parent commits if you have limited your view of history: for example, if you are only interested in changes related to a certain directory or file.

There are several built-in formats, and you can define additional formats by setting a pretty.<name> config option to either another format name, or a *format:* string, as described below (see [git-config(1)](git-config.html)). Here are the details of the built-in formats:

```
如果提交是合并，并且如果pretty-format格式不是oneline，email或raw，则在Author：
行之前插入一行。该行以“ Merge：”开头，并打印祖先提交的哈希，并用空格分隔。请注意，如果您限制了历史记录的视图，例如，如果您仅对与某个目录或文件相关的更改感兴趣，则列出的提交不一定是直接父提交的列表。
内置的格式有几种，您可以通过将pretty。<name> config选项设置为另一个格式名称或format：string来定义其他格式，如下所述（请参阅git-config（1））。以下是内置格式的详细信息
```

