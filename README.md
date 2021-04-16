# ExCmd [![Hex.pm](https://img.shields.io/hexpm/v/ex_cmd.svg)](https://hex.pm/packages/ex_cmd)

ExCmd is an Elixir library to run and communicate with external programs with back-pressure.

ExCmd is built around the idea of streaming data through an external program. Think streaming a video through `ffmpeg` to serve a web request. For example, getting audio out of a stream is as simple as

``` elixir
ExCmd.stream!(~w(ffmpeg -i pipe:0 -f mp3 pipe:1), input: File.stream!("music_video.mkv", [], 65336))
|> Stream.into(File.stream!("music.mp3"))
|> Stream.run()
```

### Major advantages over port

* Unlike beam ports, ExCmd puts back pressure on the external program
* Proper program termination. No more zombie process
* Ability to close stdin and wait for output (with ports one can not selectively close stdin)
* Stream abstraction

## Overview

ExCmd uses [odu](https://github.com/akash-akya/odu) middleware to interact with external process. It uses a demand-driven protocol for input and output to have back-pressure. There is stream abstraction `ExCmd.stream!` to simplify interaction with external process.

Refer [documentation](https://hexdocs.pm/ex_cmd/readme.html) for information

**Check out [Exile](https://github.com/akash-akya/exile) which is an alternative solution based on NIF without middleware overhead**

**Note: ExCmd is still WIP. Expect breaking changes**

## Installation
1. Install [odu](https://github.com/akash-akya/odu/releases/tag/v0.2.3) v0.2.3 and make sure its in your path
2. Install ExCmd
```elixir
def deps do
  [
    {:ex_cmd, "~> x.x.x"}
  ]
end
```

## Development

To compile `odu`

```elixir
mix compile.odu
```
