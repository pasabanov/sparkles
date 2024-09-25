# .・゜゜ 𝕊ℙ𝔸ℝ𝕂𝕃𝔼𝕊 ・゜゜・
<img src="https://img.shields.io/crates/v/sparkles"></img>
<img src="https://img.shields.io/crates/size/sparkles"></img>

Performance-focused library for capturing execution flow of application.

**What?**
Just add instant_event! macro to your code and see all events in a timeline with CPU cycle precision. \
**How?**
Fast. Blazingly fast. 🚀 Recording single event to trace buffer takes 9ns.

![img_1.png](https://github.com/skibon02/sparkles/blob/main/img_1.png?raw=true)
## ✧ Main parts
- **sparkles**: Ready-to-use library for capturing events and saving them to file in lightweight encoded format.
- **sparkles-core**: Common functionality for std and no_std (todo) version of sparkles.
- **sparkles-macro**: instant_event! and range_event_start! macro to encode event name into integer value.
- **sparkles-parser**: This binary will parse tracing data, decode events and save them to file in Perfetto protobuf format.

## ✧ How to use
1. Add sparkles as a dependency to your project
```bash
cargo add sparkles 
cargo add sparkles-macro
```
2. Add some events to your code

```rust
use std::time::Duration;
use sparkles_macro::{instant_event, range_event_start};

// Refer to sparkles/examples/how_to_use.rs
fn main() {
    let finalize_guard = sparkles::init_default();
    let g = range_event_start!("main()");

    let jh = std::thread::Builder::new().name(String::from("joined thread")).spawn(|| {
        for _ in 0..100 {
            instant_event!("^-^");
            std::thread::sleep(Duration::from_micros(1_000));
        }
    }).unwrap();
    
    let jh = std::thread::Builder::new().name(String::from("detached thread")).spawn(|| {
        for _ in 0..30 {
            instant_event!("*_*");
            std::thread::sleep(Duration::from_micros(1_000));
        }
    }).unwrap();

    for i in 0..1_000 {
        instant_event!("✨✨✨");
        std::thread::sleep(Duration::from_micros(10));
    }

    jh.join().unwrap();
}
```
3. Run your code. As it finishes, `trace/*.sprk` is generated.
4. Run sparkles-parser in directory with `trace` folder.
```bash
cargo run --example file-parser
```
5. Go to https://ui.perfetto.dev and drag'n'drop resulting `.perf` file.
6. Observe the result:
![img.png](https://github.com/skibon02/sparkles/blob/main/img.png?raw=true)


## ✧ Requirements
🌟 STD support (works better on x86 architecture)

## ✧ Benches
Single event overhead on average x86 machine (Intel i5-12400) is 9ns.

˚ ༘ ⋆｡˚ ✧ ˚ ༘ ⋆｡˚ ༘ ⋆｡˚ ✧ ˚ ༘ ⋆｡˚˚ ༘ ⋆｡˚ ✧ ˚ ༘ ⋆｡˚ ༘ ⋆｡˚ ✧ ˚ ༘ ⋆｡˚༘ ⋆｡˚ ✧ ˚ ༘\
Up to 🫸100kk🫷 events can be captured in a local environment with no data loss. \
༘ ⋆｡˚ ༘ ⋆｡˚ ✧ ˚ ༘ ⋆｡˚༘ ⋆｡˚ ✧ ˚ ༘ ⋆｡˚༘ ⋆｡˚ ✧ ˚ ༘ ⋆｡˚༘ ⋆｡˚ ✧ ˚ ༘ ⋆｡˚༘ ⋆｡˚ ✧ ˚


## ✧ Implementation status
Ready: \
🌟 Timestamp provider \
🌟 Event name hashing \
🌟 ~~Perfetto json format compatibility~~ (replaced with protobuf) \
🌟 Ranges (scopes) support \
🌟 Configuration support \
🌟 Perfetto protobuf format support \

TODO: \
⚙️ Abstraction over events sending type (TCP/UDP/IPC/File) \
⚙️ Additional attached binary data \
⚙️ Module info support: full module path, line of code \
⚙️ Capture and transfer loss detection with no corruption to other captured and transmitted data \
⚙️ Async support \
⚙️ NO_STD implementation \
⚙️ tags / hierarchy of events \
⚙️ Viewer app \
⚙️ Multi-app sync \
⚙️ Global ranges \
⚙️ Measurement overhead self-test

｡ﾟﾟ･｡･ﾟﾟ｡\
ﾟ。SkyGrel19 ✨\
　ﾟ･｡･
