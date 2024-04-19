### `tr` — Allows replacing characters with others (Translate char pos)

**Default Usage**
	`tr [old_chars] [new_chars]` 

#### ROT13 example:
*A->N,…,Z->M*
`tr 'A-Za-z' 'N-ZA-Mn-za-m'`

> In tr command, Set2 is longer because if we were to just jump from N to M it would be only 2 characters so it jumps from N to A by going through Z (N-ZA)

#cryptography
#linux/commands/text 