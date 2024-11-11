# ipapi

A Rust library to query IP addresses using the [ipquery.io](https://ipquery.io) API.

## Features

- Query details for a specific IP address
- Bulk query multiple IP addresses
- Fetch your own public IP address

## Installation

To use this crate, add the following to your `Cargo.toml`:

```toml
[dependencies]
ipapi = "0.1.0"
tokio = { version = "1.0", features = ["full"] }
```

## Usage

### Query a Specific IP Address

The `query_ip` function retrieves information about a specific IP address, including its ISP, location, and risk data.

```rust
use ipapi::query_ip;
use tokio;

#[tokio::main]
async fn main() {
    let ip_info = query_ip("8.8.8.8").await.unwrap();
    println!("{:#?}", ip_info);
}
```

#### Output Example
```plaintext
IPInfo {
    ip: "8.8.8.8",
    isp: Some(ISPInfo { asn: Some("AS15169"), org: Some("Google LLC"), isp: Some("Google LLC") }),
    location: Some(LocationInfo {
        country: Some("United States"),
        country_code: Some("US"),
        city: Some("Mountain View"),
        state: Some("California"),
        zipcode: Some("94035"),
        latitude: Some(37.386),
        longitude: Some(-122.0838),
        timezone: Some("America/Los_Angeles"),
        localtime: Some("2024-11-09T12:45:32"),
    }),
    risk: Some(RiskInfo {
        is_mobile: Some(false),
        is_vpn: Some(false),
        is_tor: Some(false),
        is_proxy: Some(false),
        is_datacenter: Some(true),
        risk_score: Some(0),
    }),
}
```

### Bulk Query Multiple IP Addresses

The `query_bulk` function allows you to query information for multiple IP addresses at once.

```rust
use ipapi::query_bulk;
use tokio;

#[tokio::main]
async fn main() {
    let ip_infos = query_bulk(&["8.8.8.8", "1.1.1.1"]).await.unwrap();
    for info in ip_infos {
        println!("{:#?}", info);
    }
}
```

#### Output Example
```plaintext
IPInfo {
    ip: "8.8.8.8",
    ...
}
IPInfo {
    ip: "1.1.1.1",
    ...
}
```

### Fetch Your Own Public IP Address

The `query_own_ip` function retrieves the public IP address of the current machine.

```rust
use ipapi::query_own_ip;
use tokio;

#[tokio::main]
async fn main() {
    let ip = query_own_ip().await.unwrap();
    println!("Your IP Address: {}", ip);
}
```

#### Output Example
```plaintext
Your IP Address: 203.0.113.45
```

## API Documentation

### 1. `query_ip`

#### Signature
```rust
pub async fn query_ip(ip: &str) -> Result<IPInfo, reqwest::Error>
```

#### Description
Fetches detailed information about a specific IP address, including its ISP, location, and risk information.

#### Parameters
- `ip`: A string slice representing the IP address to query.

#### Returns
- `Ok(IPInfo)` on success, containing details about the IP address.
- `Err(reqwest::Error)` if the network request or JSON deserialization fails.

#### Example
```rust
let result = query_ip("8.8.8.8").await?;
println!("{:?}", result);
```

---

### 2. `query_bulk`

#### Signature
```rust
pub async fn query_bulk(ips: &[&str]) -> Result<Vec<IPInfo>, reqwest::Error>
```

#### Description
Fetches information for multiple IP addresses at once. Useful for batch processing.

#### Parameters
- `ips`: A slice of string slices representing the list of IP addresses to query.

#### Returns
- `Ok(Vec<IPInfo>)` on success, containing details for each IP address.
- `Err(reqwest::Error)` if the network request or JSON deserialization fails.

#### Example
```rust
let ips = ["8.8.8.8", "1.1.1.1"];
let results = query_bulk(&ips).await?;
println!("{:?}", results);
```

---

### 3. `query_own_ip`

#### Signature
```rust
pub async fn query_own_ip() -> Result<String, reqwest::Error>
```

#### Description
Fetches the public IP address of the current machine. Useful for determining your own IP address.

#### Returns
- `Ok(String)` containing the public IP address.
- `Err(reqwest::Error)` if the network request fails.

#### Example
```rust
let ip = query_own_ip().await?;
println!("Your IP Address: {}", ip);
```

---

## Running Tests

To run the tests for this crate:

```bash
cargo test
```

## Contributing

Contributions are welcome! Feel free to open issues or submit pull requests on the [GitHub repository](https://github.com/ipqwery/ipapi).

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.
