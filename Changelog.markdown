# 2018-06-04 (v0.4)

* add contributed feature to allow users to [create arbitrary relations](https://github.com/agentm/project-m36/blob/master/docs/tutd_tutorial.markdown#arbitrary-relation-variables)
* fix type validation bug allowing concrete type with type variables in relations
* improve usability of `Interval a` data type
* allow `Integer` to be parsed as negative in `tutd` console
* support precompiled (Haskell) [`AtomFunction`s](https://github.com/agentm/project-m36/blob/master/docs/atomfunctions.markdown#pre-compiled-atom-functions) and [`DatabaseContextFunction`s](https://github.com/agentm/project-m36/blob/master/docs/database_context_functions.markdown#loading-precompiled-modules)
* make TutorialD scripts read from the filesystem unconditionally read as UTF-8
* improve support for display of multibyte characters in `tutd` console, especially Chinese
* fix file descriptor leak in file sychronization
* improve reliability by allowing fast-forward commits when using `autoMergeToHead`
* add [`semijoin` and `anitjoin`](https://github.com/agentm/project-m36/blob/master/docs/tutd_tutorial.markdown#join) support to `tutd`
* added various new static optimizations

# 2017-11-14

* alter websocket server API to allow for multiple representations (JSON, text, or HTML) to be selected and returned simultaneously
* add jupyter kernel for TutorialD interpreter
* fix warnings suggested by new hlint 2.0.10	
	
# 2017-10-08 (v0.3)

* replaced overuse of `undefined` with `Proxy` in `Tupleable` and `Atomable` typeclasses
* allow notifications to return query results from before *and* after the commit which triggered the notification
* alert users in `tutd` console before a transaction graph expression is evaluated which would throw out their changes
* drastically-improved CSV import/export now supports all possible types except `RelationAtom`
* fix serious file handle leaks when using on-disk persistence
* fix case where invalid number of arguments to `ConstructedAtom` did not result in an error
* add support for `IntegerAtom` (previously, only `IntAtom` was supported)

# 2017-09-16 (v0.2)

* a new [simple client API](https://github.com/agentm/project-m36/blob/master/docs/simple_api.markdown) with a monadic transaction manager
* complete hlint compliance
* the generics-based [Tupleable typeclass](	https://github.com/agentm/project-m36/blob/master/docs/tupleable.markdown
) which makes it easy to marshal Haskell data types to-and-from the database
* timestamps attached to transactions to allow specific point-in-time travel
* autoMergeToHead, a variant of commit which attempts to merge and commit to the latest head transaction to reduce incidents of TransactionNotAHeadErrors
* interval data types
* transaction dirtiness detection which allows the client to determine if an update expression actually changed the database state

# 2017-08-01

## autoMergeToHead

In preparation for the simpler monad client API, ProjectM36.Client now includes a server-side merge for new transactions called "[automerge](https://github.com/agentm/project-m36/issues/33)". This feature should reduce head contention in cases where new transactions can be simply merged to the head without additional processing. The trade-off is reduced ```TransactionIsNotAHeadError```s but an increased chance of merge errors. The feature operates similarly to a server-side git rebase.

## critical bug in merging

Successfully merged transactions did not have their constraints validated. Fixed.

# 2017-06-12

## add file locking

This [feature](#102) allows Project:M36 database directories to be shared amongst multiple Project:M36 processes. This is similar to how SQLite operates except that the remote server mode supports the feature as well. This could allow, for example, multi-master, file-based replication across Windows shares or NFS.

[Documentation](/docs/replication.markdown)

# 2016-11-30

## add functional dependency macro

Date demonstrates two ways to implement functional dependencies as constraints on page 21 in "Database Design and Relational Theory". A similar macros is now implemented in the tutd interpreter.

```funcdep sname_status (sname) -> (status) s```

[Documentation](/docs/tutd_tutorial.markdown#functional-dependencies)

# 2016-09-07

## add TransGraphRelationalExpr

The TransGraphRelationalExpr allows queries against all past states of the database.

The following example executes a query against two different committed transactions using syntax similar to that of git for graph traversal:
```:showtransgraphexpr s@master~ join sp@master```

[Documentation](/docs/transgraphrelationalexpr.markdown)
