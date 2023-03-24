# Halp

## ChatGPT powered terminal helper

### Installation:

Copy `halp` somewhere in your path.

Add an environment variable `OPENAI_API_KEY`. The value of this variable should be your API key from OpenAI.
https://platform.openai.com/account/api-keys

You can now use halp:

```
halp compile test.c on macos, using C99 and optimization level 3. The output file should be called program and should be a freestanding executable  | bash

0:: Cancel
1:: clang -std=c99 -O3 -o program test.c
Select, or zero (or non-number) to cancel [0, 1..1]:
```

When called, halp will go to ChatGPT for command suggestions, and will present them to you, allowing you to choose
which you want to run - you'll also have the ability to cancel. This means you can you can also directly pipe the output 
to other commands, e.g. `bash`, since this will only be called if you select an option - if you cancel then the pipeline will terminate.

```
halp a bash command that echoes 'Yolo' to the terminal 5 times | bash

0:: Cancel
1:: for run in {1..5}; do echo "Yolo"; done
Select, or zero (or non-number) to cancel [0, 1..1]:

Yolo
Yolo
Yolo
Yolo
Yolo
```

It knows how to run common commands (but doesn't always get it right :D):

```
halp 'mysql command to connect as root to the database test and show all rows from the ledger table. Dont include the -p option' | bash

0:: Cancel
1:: mysql -u root test -e "SELECT * FROM ledger;"
Select, or zero (or non-number) to cancel [0, 1..1]:

id	amt	line	balance_before
1	40	Spend some of bonus	1000
2	50	yolo	50
3	30	Spend more of bonus	960
```

And can directly generate non-bash commands (e.g. SQL):

```

halp SQL to show all rows in the bonus table and related rows in the ledger table, using the ledger_bonus join table and primary keys named "id" in ledger and bonus | mysql -u root -D test

0:: Cancel
1:: SELECT *
FROM bonus
JOIN ledger_bonus ON bonus.id = ledger_bonus.bonus_id
JOIN ledger ON ledger.id = ledger_bonus.ledger_id;
Select, or zero (or non-number) to cancel [0, 1..1]:

id	type	opening_bal	ledger_id	bonus_id	id	amt	line	balance_before
1	CoolBonus	500	1	1	1	40	Spend some of bonus	1000
1	CoolBonus	500	3	1	3	30	Spend more of bonus	960
``
`
