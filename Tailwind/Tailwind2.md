# Colors
\<utility> - \<color> - \<step/opacity>
Ex: bg-blue-50/10

| step:    | 50, 100 ... 900, 950 |                        |
| -------- | -------------------- | ---------------------- |
| opacity: | 10 ... 100           | (--var) \|\| \[71.37%] |

# Background
همه یوتیلیتی های background با bg شروع می شوند.
## Gradiant
| -   | bg- | linear |                                         | - \<angle> \|\| (--var) \|\| [value] |
| --- | --- | ------ | --------------------------------------- | ------------------------------------ |
|     | bg- | radial | -to - { t,tr,tl \|\| b,br,bl \|\| r,l } | - \<angle> \|\| (--var) \|\| [value] |
| -   | bg- | conic  |                                         | - \<angle> \|\| (--var) \|\| [value] |
$from || via || to - \frac{<color>}{<percentage>} || \frac{(--var)}{<angle>}$
Ex:`from-10% via-30%  to-90%`
## Image
$bg - \frac{[url(/img/mypic.jpg)]}{(image:(--filePath-Var>))}$






Make a note of something, [[create a link]], or try [the Importer](https://help.obsidian.md/Plugins/Importer)!

When you're ready, delete this note and make the vault your own.