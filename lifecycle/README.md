# lifecycle

Rust implementation of https://github.com/stuartsierra/component.

This library only contains a trait:

```rust
pub trait Lifecycle: Sized {
    fn start(self) -> Self {
        self
    }
    fn stop(self) -> Self {
        self
    }
}
```

It shines better using the crate `derive-system`, you can use both like this:

## Usage

```toml
[dependencies]
lifecycle = "0.1.0"
system-derive = "0.1.0"
```

## Example

```rust
use derive_system::System;
use lifecycle::Lifecycle;

struct App;
impl Lifecycle for App {
    fn start(self) -> Self {
        println!("App::start");
        Self
    }

    fn stop(self) -> Self {
        println!("App::stop");
        Self
    }
}

struct Scheduler;
impl Lifecycle for Scheduler {
    fn start(self) -> Self {
        println!("Scheduler::start");
        Self
    }

    fn stop(self) -> Self {
        println!("Scheduler::stop");
        Self
    }
}

struct Database;
impl Lifecycle for Database {
    fn start(self) -> Self {
        println!("Database::start");
        Self
    }

    fn stop(self) -> Self {
        println!("Database::stop");
        Self
    }
}

#[derive(System)]
pub struct ExampleSystem {
    app: App,
    scheduler: Scheduler,
    database: Database,
}

fn main() {
    let mut system = ExampleSystem {
        app: App,
        scheduler: Scheduler,
        database: Database,
    };

    system = system.start();

    let _ = system.stop();
}
```

This outputs:

```
App::start
Scheduler::start
Database::start
Database::stop
Scheduler::stop
App::stop
```
