## v0.9.2 to-be-released

### Changed

- [fixed #306] Inheriting from a misconfigured adapter relation will raise a
  meaningful error (solnic)

## v0.9.1 2015-08-21

This is a small bug-fix release which addresses a couple of issues for inline
setup DSL and multi-environments.

### Fixed

- Multi-env setup for adapters with schema-inferration support won't crash (solnic)
- Default adapter is set correctly when many adapters are configured and one is
  registered under `:default` name (solnic)
- When defining a relation using inline DSL with custom dataset name the relation
  name will be correctly set as `register_as` setting (solnic)

### Changed

- When using inline-setup for env the auto_registration mechanism will be turned
  on by default (solnic)

[Compare v0.9.0...v0.9.1](https://github.com/rom-rb/rom/compare/v0.9.0...v0.9.1)

## v0.9.0 2015-08-19

### Added

* Configuration API for gateways supporting following options:
  - `infer_relations` either `true` or `false` - if disabled schema inference
    won't be used to automatically set up relations for you
  - `inferrable_relations` a list of allowed relations that should be inferred
  - `not_inferrable_relations` a list of relations that should not be inferred

### Changed

* Global setup with auto-registration ported to the `:auto_registration` environment plugin (AMHOL)
* Multi-environment setup possible now via `ROM::Environment` object (AMHOL)
* All relations are now lazy with auto-currying enabled (solnic)
* Low-level query DSL provided by adapters is now public but using it directly in
  application layer is discouraged (solnic)
* `ROM::Mapper` component extracted into standalone `rom-mapper` gem (solnic)
* Support libraries extracted to `rom-support` gem (solnic)

## Fixed

* `register_as` is now properly inferred for relations and their descendants (solnic)
* Adapter-specific interface is properly included in relation descendants (solnic)
* Combined commands (aka command graph) properly rejects keys from nested input
  prior sending the input to individual commands (solnic)
* Composite relation materializes correctly when another composite on the right
  side became materialized (ie piping relation through a composite relation will
  work correctly) (solnic)

[Compare v0.8.1...v0.9.0](https://github.com/rom-rb/rom/compare/v0.8.1...v0.9.0)

## v0.8.1 2015-07-12

### Fixed

* `ROM::CommandError` properly sets original error and backtrace (solnic)

### Changed

* Internal transproc processor has been updated to the new API (solnic)

[Compare v0.8.0...v0.8.1](https://github.com/rom-rb/rom/compare/v0.8.0...v0.8.1)

## v0.8.0 2015-06-22

### Added

* Commands can be combined into a single command that can work with a nested input (solnic)
* New `step` mapper operation that allows multistep transformations inside a single mapper (dekz)
* New `ungroup` and `unfold` mapper operations inverse `group` and `fold` (nepalez)
* Support deep nesting of `unwrap` mapper operations (nepalez)
* Support usage of `exclude` in a root of the mapper (nepalez)
* Support usage of `prefix` and `prefix_separator` mapper operations inside blocks (nepalez)
* Support renaming of the rest of an attribute after `unwrap` (nepalez)

### Changed

* `Repository` class has been renamed to `Gateway` with proper deprecation
  warnings (cflipse)
* `combine` in mapper can be used without a block (kwando)
* `wrap` and `group` in mapper will raise error if `:mapper` is set along with
  block or options (vrish88)

### Fixed

* `order` memory repository operation sorts tuples containing empty values (nepalez)
* `Mapper::AttributeDSL#embedded` now honors `option[:type]` when used
  with `option[:mapper]` (c0)

[Compare v0.7.1...v0.8.0](https://github.com/rom-rb/rom/compare/v0.7.1...v0.8.0)

## v0.7.1 2015-05-22

### Added

* Support for passing a block for custom coercion to `attribute` (gotar)
* `fold` mapping operation which groups keys from input tuples to array
  of values from the first of listed keys (nepalez)
* Adapter `Relation` and command classes can specify `adapter` identifier
  which allows using adapter-specific plugins w/o the need to specify adapter
  when calling `use` (solnic)

### Changed

* [rom/memory] `restrict` operation supports array as a value (gotar)
* [rom/memory] `restrict` operation supports regexp as a value (gotar)

[Compare v0.7.0...v0.7.1](https://github.com/rom-rb/rom/compare/v0.7.0...v0.7.1)

## v0.7.0 2015-05-17

### Added

* `combine` interface in Relation and Mapper which allows simple and explicit
  eager-loading that works with all adapters (solnic)
* `reject_keys` option in mapper which will filter out unspecified keys from
  input tuples (solnic)
* `unwrap` mapping operation (aflatter)
* Arbitrary objects can be registered as mappers via `register` in mapping DSL (solnic)
* Ability to reuse existing mappers in `group`, `wrap` and `embedded` mappings (solnic)
* Plugin interface for Relation, Mapper and Command (cflipse)
* `Memory::Dataset` accepts options hash now which makes it more flexible for
  any adapter that wants to subclass it (solnic)
* `ROM::Memory::Relation#take` (solnic)

### Changed

* [BREAKING] `Command#call` applies curried args first (solnic)
* `Commands::Update#set` was deprecated in favor of `call` (solnic)
* `group` mapping reject empty tuples (solnic)

### Fixed

* `Command` respond to missing properly now (solnic)
* `Mapper::DSL` respond to missing properly now (solnic)

### Internal

* Fixed all the warnings \o/ (splattael)
* Introduced `Deprecations` helper module (solnic)

[Compare v0.6.2...v0.7.0](https://github.com/rom-rb/rom/compare/v0.6.2...v0.7.0)

## v0.6.2 2015-04-14

### Changed

* Updated to transproc 0.2.0 (solnic)

### Fixed

* `CommandRegistry#respond_to_missing?` behavior (hecrj)

[Compare v0.6.1...v0.6.2](https://github.com/rom-rb/rom/compare/v0.6.1...v0.6.2)

## v0.6.1 2015-04-04

### Added

* Ability to auto-map command result via `rom.command(:rel_name).as(:mapper_name)` (solnic)

### Changed

* gemspec no longer specifies required_ruby_version so that rom can be installed on jruby (solnic)
* Obsolete `Env#readers` was removed (splattael)

[Compare v0.6.0...v0.6.1](https://github.com/rom-rb/rom/compare/v0.6.0...v0.6.1)

## v0.6.0 2015-03-22

### Added

* It is now possible to define custom relation, mapper and command classes during setup (solnic)
* New `Env#relation` interface for reading and mapping relations which supports:
  * `Relation::Lazy` with auto-currying, mapping and composition features (solnic)
  * `Relation::Composite` allowing data-pipelining with arbitrary objects (solnic)
  * Passing a block which yields relation with adapter query DSL available (solnic)
* Relations can be extended with plugins using Options API (solnic)
* Commands are now composable via `>>` operator (solnic)
* Mappers support `prefix_separator` option (solnic)
* Mappers can be registered under custom names (solnic)
* Relation `dataset` name is inferred from the class name by default (gotar)
* Relation can be registered under a custom name via `register_as` option (mcls)
* Adapters can use helper modules for datasets: `ArrayDataset` and `EnumerableDataset` (solnic)
* Adapter interface can now be tested via a lint test (elskwid + solnic + splattael)
* `tuple_count` interface in AbstractCommand which can be overridden by adapter (solnic)
* Custom Inflector API that auto-detects a specific inflection engine (mjtko)

### Changed

* [BREAKING] Schema DSL was **removed** - attributes can be specified only in mapper DSL
* [BREAKING] Reader was **removed** in favor of relation interface with explicit mapping (solnic)
* [BREAKING] Command API was simplified - commands should be accessed directly in `.try` block
  and default repository can be changed when defining a relation (solnic)
* `.setup` interface requires either an adapter identifier or can accept a repository
  instance (aflatter)
* Adapter interface no longer requires specific constructor to be defined (aflatter)
* Adapters no longer need to handle connection URIs (aflatter)
* Adapter/Repository has been collapsed to *just* `Repository` (solnic)
* Relation no longer needs a header object and only operates on an adapters dataset (solnic)
* Relation no longer uses on Charlatan with method_missing magic (solnic)
* Adapter's dataset no longer requires header (solnic)
* Make storage in memory adapter thread-safe #110 (splattael)
* An Adapter can provide its own Relation subclass with custom behavior (solnic)
* Relation provides its "public interface" using method_added hook (splattael + solnic)
* ROM no longer depends on charlatan, concord and inflecto gems (mjtko + solnic)

[Compare v0.5.0...v0.6.0](https://github.com/rom-rb/rom/compare/v0.5.0...v0.6.0)

## v0.5.0 2014-12-31

### Added

* Mapper DSL supports `embedded` interface for nested tuples (solnic)
* Support for nested `group` mapping (solnic)
* Support for nested `wrap` mapping (solnic)
* Support for primitive type coercions (:to_string, :to_integer etc.) (solnic)
* Support for top-level `:prefix` option in mapping DSL (solnic)
* Support for top-level `:symbolize_keys` option in mapping DSL (solnic)
* Support for `:prefix` option in wrap/group mapping DSL (solnic)
* Interface for registering data mapping processors (solnic)
* Remaining relations are automatically setup from the schema (solnic)
* Each relation has now access to other relations (previously they only had
  access to raw datasets) (solnic)
* `ROM.setup` supports passing in *just an uri* which will setup a default repository (solnic)
* `ROM.setup` supports passing in conventional database connection hash (solnic)
* Adapters support extra options in addition to the base connection URI (solnic)

### Changed

* Mapping backend replaced by integration with transproc (solnic)
* Readers no longer expose adapter query DSL (solnic)
* Registry objects raise `ROM::Registry::ElementNotFoundError` when missing
  element is referenced (rather than raw KeyError) (solnic)
* Performance improvements in Reader (solnic)
* `ROM::RA` was merged into in-memory adapter as this fits there perfectly (solnic)
* It is no longer needed to explicitly execute a delete command in try block (solnic)

### Fixed

* Wrap/group skips empty tuples now (solnic)
* Readers raise a meaningful error when relation is missing (solnic)

## Internal

* Massive code clean-up and rubocop integration (chastell)
* Refactored `Reader` and mapper-specific logic into `MapperRegistry` (solnic)

[Compare v0.4.2...v0.5.0](https://github.com/rom-rb/rom/compare/v0.4.2...v0.5.0)

## v0.4.2 2014-12-19

### Added

* Mappers support tuple transformation using wrap and group operations (solnic)
* PORO model builder supports namespaced constants via `name: 'MyApp:Entities::User` (solnic)

### Changed

* `ROM::RA` interface is no longer mixed into relations by default (solnic)
* ~2.5 x speed up in aggregate mapping (solnic)
* PORO model builder only defines attribute readers now (no writers!) (solnic)
* Registry objects in Env will now raise `KeyError` when unknown name is referenced (solnic)

[Compare v0.4.1...v0.4.2](https://github.com/rom-rb/rom/compare/v0.4.1...v0.4.2)

## v0.4.1 2014-12-15

### Added

* Adapter can now implement `Adapter#dataset(name, header)` to return a dataset (solnic)
* For multi-step setup the DSL is available in `ROM` too (solnic)
* Global environment can be stored via `ROM.finalize` and accessible via `ROM.env` (solnic)
* Mapper won't infer attributes from the header if `:inherit_header` option is set to false (solnic)

### Changed

* Schema can be defined in multiple steps (solnic)
* Setting model in mapper DSL is no longer required and defaults to `Hash` (solnic)
* Adapter datasets no longer have to return headers when they are provided by schema (solnic)

[Compare v0.4.0...v0.4.1](https://github.com/rom-rb/rom/compare/v0.4.0...v0.4.1)

## v0.4.0 2014-12-06

### Added

* Command API (solnic)
* Setup DSL is now available within the `ROM.setup` block (solnic)
* Support for setting up a logger for an adapter (solnic)
* New `Adapter#dataset?(name)` which every adapter must implement (solnic)

### Fixed

* method-missing in `Repository` and `Env` kindly calls `super` (solnic)

### Changed

* Abstract `Adapter` defines `:connection` reader so it doesn't have to be
  defined in adapter descendants (solnic)

[Compare v0.3.1...v0.4.0](https://github.com/rom-rb/rom/compare/v0.3.1...v0.4.0)

## v0.3.1 2014-11-25

### Added

* attributes for `group` mapping operation can be specified without options (solnic)
* attributes for `wrap` mapping operation can be specified without options (solnic)
* `Env` uses Equalizer (solnic)
* boot dsl methods return self (solnic)

### Fixed

* when schema is missing booting will gracefuly skip building relations and mappers (solnic)
* in-memory join handles one-to-many and many-to-one correctly (solnic)

[Compare v0.3.0...v0.3.1](https://github.com/rom-rb/rom/compare/v0.3.0...v0.3.1)

## v0.3.0 2014-11-24

This version is a rewrite that introduces a new, simplified architecture based
on a new adapter interface.

[Compare v0.2.0...v0.3.0](https://github.com/rom-rb/rom/compare/v0.2.0...v0.3.0)

## v0.2.0 2014-04-06

### Added

* [feature] added :rename option to schema attribute DSL (solnic)
* [feature] added support for join, group, wrap, project and rename operations (solnic)
* [feature] added support for setting domain object loading strategy (solnic)
* [feature] Environment.setup can be used with a block to define schema and mapping (solnic)
* [feature] added public interface for building mappers (see Mapper.build) (solnic)
* [feature] added support for mapping embedded objects using wrap/group (solnic)
* [feature] environment exposes mapper registry via Environment#mappers (solnic)

### Changed

* [BREAKING] rom-relation, rom-mapper and rom-session were merged into rom project (solnic)
* [BREAKING] changed mapping DSL (users do...end => relation(:users) do...end) (solnic)
* [BREAKING] added :from option to mapping DSL which replaced :to (solnic)
* [internal] mappers are now backed by [morpher](https://github.com/mbj/morpher) (solnic)
* [internal] renaming and optimizing relations happens on the schema level now (solnic)
* [internal] environment will raise if unknown relation is referenced via `Environment#[]` (solnic)

[Compare v0.1.2...v0.2.0](https://github.com/rom-rb/rom/compare/v0.1.2...v0.2.0)

## v0.1.2 2013-09-02

* [updated] [rom-relation](https://github.com/rom-rb/rom-relation/blob/v0.1.2/Changelog.md#v012-2013-09-02)

[Compare v0.1.1...v0.1.2](https://github.com/rom-rb/rom/compare/v0.1.1...v0.1.2)

## v0.1.1 2013-08-30

* [updated] [rom-relation](https://github.com/rom-rb/rom-relation/blob/v0.1.1/Changelog.md#v011-2013-08-30)
* [updated] [rom-session](https://github.com/rom-rb/rom-session/blob/v0.1.1/Changelog.md#v011-2013-08-30)

[Compare v0.1.0...v0.1.1](https://github.com/rom-rb/rom/compare/v0.1.0...v0.1.1)

## v0.1.0 2013-08-23

First public release
