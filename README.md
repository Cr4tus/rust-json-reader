# Rust JSON Reading Experiments

## What does the app do?
The program makes simple prints of json data before and after *serialization* / *deserialization*.

## Programming Languages:
- Rust

## Frameworks & Libraries:
- serde - https://serde.rs | https://lib.rs/crates/serde | https://github.com/serde-rs/serde
- serde-json - https://github.com/serde-rs/json

## Implementation:
First, we declare a json object as a string:
```rust
let json = r#"
{
    "title": "JSON API paints my bikeshed!",
    "author": "Jordan Harband",
    "paragraphs": [
        {
            "content": "This is where my bike lived."
        },
        {
            "content": "And this is where my bike bought something."
        },
        {
            "content": "And this is where my bike bought something too."
        },
        {
            "content": "And this is where my bike lived."
        }
    ]
}"#;
```
This represents a serialized Article, which we have defined (and annotated with serde's annotations) before the *main* function:
```rust
#[derive(Serialize, Deserialize, Debug)]
struct Article {
    title: String,
    author: String,
    paragraphs: Vec<Paragraph>
}
```
After this, we deserialize the data using a utility function also defined at the top of the *main.rs* file:
```rust
fn deserialize <'a, T> (serialized_data: &'a str) -> std::option::Option<T> where T: Deserialize <'a> {
    match serde_json::from_str::<T>(serialized_data) {
        Ok(data) => Some(data),
        Err(_) => None
    }
}
```
It requires **lifetime annotation** because it deals with borrowed data and needs to ensure that the references it holds remain valid for the duration of the its execution. 