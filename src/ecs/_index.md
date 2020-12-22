# Entities, Components and Systems

At its heart, Bevy operates using an [Entity-Component-System](https://en.wikipedia.org/wiki/Entity_component_system) model. Rather than operating in a traditional object-oriented fashion, where your game is composed of individual objects with properties and methods, ECS games work in a fashion that is much closer to a relational database: grouping similar data together in a memory-dense fashion, with the `Entity` serving as a primary key to identify the data, then processing the game data in a batch-wise fashion.

Games in Bevy are largely composed of 5 core building blocks, that come up again and again:
- **Entity:** [Entities](https://docs.rs/bevy/0.4.0/bevy/ecs/struct.Entity.html) are a simple, unique identifier: signifying which in-game object the data stored in a component belongs to. If you need to refer to another object in your game, use its `Entity`
- **Components:** [Components](https://docs.rs/bevy/0.4.0/bevy/ecs/trait.Component.html) refer to data that is associated with an entity. You can use any thread-safe Rust type as a component, and they are stored in a performant fashion using [archetypes](internals/archetypes.md).
- **Systems:** [Systems](https://docs.rs/bevy/0.4.0/bevy/ecs/trait.System.html) are largely-ordinary Rust function that handles the game logic. They [query](../../book/ecs/queries.html) for components to operate on and can read in common resources (including [Commands](communication/commands.md) and [Events](communication/events.md). When added to your game, they automatically fetch data of the appropriate type, and are run in parallel by the [scheduler](timing/scheduling.md) based on the resources and components that they read and write.
- **Resources:** [Resources](https://docs.rs/bevy/0.4.0/bevy/ecs/trait.Resource.html) are well-behaved global singletons that are used to store cross-system game state that isn't associated with a particular entity. Like with components, you can use any thread-safe Rust type as a Resource. 
- **Assets:** [Assets](https://docs.rs/bevy/0.4.0/bevy/asset/struct.Assets.html) are blocks of data that are used to create content for your game. Game models, raster graphics for your UI, sound effects and so on are all assets.

## Getting Started

To get started, [Install Rust](https://www.rust-lang.org/tools/install), then set up a new Bevy project ("Add Bevy as a Dependency") as described in the [official book]https://bevyengine.org/learn/book/getting-started/).

The boilerplate needed to make your own app is simple:

```rust
use bevy::prelude::*;

fn main() {
    App::build().run();
}
```

If you're looking for some useful functionality out of the box (windowing, graphics, input, a scheduler that automatically loops etc.), you'll want to add [`DefaultPlugins`](https://docs.rs/bevy/0.4.0/bevy/struct.DefaultPlugins.html) as well.

```rust
use bevy::prelude::*;

fn main() {
	App::build()
        .add_plugins(DefaultPlugins)
		.run();
}
```

By chaining methods together on the [`AppBuilder`](internals/app-builder.md) that we create, we can carefully build up our game's functionality in a modular fashion with the [Builder pattern](https://refactoring.guru/design-patterns/builder). 

To give you a taste of how all these things fit together, check out the extensive [examples](https://github.com/bevyengine/bevy/tree/master/examples) section of the official repo, or the examples and tutorials found at [awesome-bevy](https://github.com/bevyengine/awesome-bevy#games).