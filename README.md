# Reports Generator

## Proposal
The proposal for this project is a slightly sowed study on Elixir and its functionalities.<br>

In it, I'm exploring:
- Reading files
- Native functions
- Pattern Matching
- Guards
- Parallelism

### ReportsGenerator.build/1
When running, it returns 1 map with 2 key-values: "Users" and "Foods", which contains how many requests the user did and the number of requests of that food, respectively.

```elixir
iex> ReportsGenerator.build("report_test.csv")
...> %{"foods" => %{"açaí" => 1, ...}, "users" => %{"1" => 48, ...}}
```

### ReportsGenerator.build_from_many/1
Same of `build/1`, but waits for a list and builds them in parallel.

#### Success
```elixir
iex> ReportsGenerator.build_from_many(["report_test.csv", "report_test.csv"])
...> %{:ok, "foods" => %{"açaí" => 2, ...}, "users" => %{"1" => 96, ...}}
```

#### Error
```elixir
iex> ReportsGenerator.build_from_many("banana")
...> {:error, "Please provide a list of strings"}
```

### ReportsGenerator.fetch_higher_cost/2
Returns the highest value between an "user" or a "food" depending on the option that is passed.

#### Success
For Users:
```elixir
iex> "report_test.csv" |> ReportsGenerator.build() |> ReportsGenerator.fetch_higher_cost("users")
...> {:ok, {"5", 49}}
```
For Foods:
```elixir
iex> "report_test.csv" |> ReportsGenerator.build() |> ReportsGenerator.fetch_higher_cost("foods")
...> {:ok, {"esfirra", 3}}
```

#### Error
```elixir
iex> "report_test.csv" |> ReportsGenerator.build() |> ReportsGenerator.fetch_higher_cost("bananas")
...> {:error, "Invalid option!"}
```

### ReportsGenerator.Parser.parse_file/1

```elixir
iex> "report_test.csv" |> Parser.parse_file() |> Enum.map(& &1)
...> [["1", "pizza", 48], ...]
```

## Tests

This project has tests, just enter the command below at your terminal, in the same folder in this project.
```sh
.../reports_generator> mix test
```