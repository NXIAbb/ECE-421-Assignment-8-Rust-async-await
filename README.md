Download link :https://programming.engineering/product/ece-421-assignment-8-rust-async-await/


# ECE-421-Assignment-8-Rust-async-await
ECE 421 Assignment 8: Rust async/await
Question 1: Consider the following attempt to model a simple server:

use std::io;

use tokio::time::{sleep, Duration};

3 use chrono::Utc;

7

8 use std::collections::HashMap;

9

10 async fn handle_cmd(cmd: char) {

11match cmd {

12

‘i’ =>

{

13

if let Ok(r) = reqwest::get(“https://httpbin.org/ip”).await {

14

if let Ok(ip) = r.json::<HashMap<String,String>>().await {

15

println!(“{:?}”, ip);

16

}

17

}

},

‘d’ => {

20

if let Ok(r) = reqwest::get(“https://httpbin.org/delay/9”)

.await {

21

println!(“{:#?}”, r);

22

}

},

other => println!(“unknown cmd: {}”, other),

}

}

27

async fn read_cmd() -> Result<usize, Box<dyn std::error::Error>> {

let mut data = String::new();

let stdin = in::stdin();

match stdin.read_line(&mut data) {

Ok(len) => {

33

if len > 0 {

34

if let Some(cmd) = data.chars().next() {

36

handle_cmd(cmd).await;

37

}

38

}

39

Ok(len)

},

Err(e) => Err(Box::new(e)),

}

}

44

async fn periodic_task(interval: u64) {

sleep(Duration::from_secs(interval)).await;

println!(“UTC now: {}”, Utc::now());

}

50

#[tokio::main]

async fn main() -> Result<(), Box<dyn std::error::Error>> {

loop {

match read_cmd().await {

