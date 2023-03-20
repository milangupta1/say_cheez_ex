# SayCheezEx 📸  

Captures a snapshot of the environment at build time, so you can display it at run-time.

Sometimes you'd want to reference the version of your package at run time, or when / where / from what sources it was built, but that information is not available anymore once you deploy your app somewhere else.

It can also be useful to run a pipeline once when building, e.g. compiling a SASS file into CSS and storing the result as a string.

This library is heavily ispired by my previous (and very useful) Clojure library https://github.com/l3nz/say-cheez


[![Hex.pm](https://img.shields.io/hexpm/v/say_cheez_ex)](https://hex.pm/packages/say_cheez_ex)
![Hex.pm](https://img.shields.io/hexpm/dt/say_cheez_ex)
![Hex.pm](https://img.shields.io/hexpm/l/say_cheez_ex)



## Installing

Just add to your `mix.exs` file:

        {:say_cheez_ex, "~> 0.1"}


- Full documentation: https://hexdocs.pm/say_cheez_ex
- Hex.pm: https://hex.pm/packages/say_cheez_ex

## Using

Whenever you want to reference a version/build information,
create an Elixir attribute for the module and compute its value through SayCheezEx.

I often compute a "short" version number,
an User-Agent for performing HTTP requests,
and a long version that contains the full
build information.

For example:

```
module Foo do
        import SayCheezEx, only: [cheez!: 1]
        # e.g. "v 0.1.5/d9a87c3 137 on server.local"
        @version cheez!("v {:project_version}/{:git_commit_id} {:build_number} on {:build_on}")
      
        # MyProject-0.1.1
        @user_agent cheez!("{:project_name}-{:project_version}")

        ...
end
```

Always make sure that you assign those values to an attibute - **never call those functions directly**.

You can safely create such attributes in all modules that need them, as they are just one (usually very small) binary.

Strings composed through `cheez!` will interpolate attributes
between brackets, with the following rules:

- `{:project_version}` is an info tag. These is a long 
   list of those - see below.
- `{$HOST}` is the environment variable HOST
- `{=HELLO}` is a default value, in this case the literal string "HELLO"
- If multiple attributes are specified, they all are expanded,
  and the first one that is defined will be output. So e.g.
  `{$FOO,$BAR,=BAZ}` will first try to interpolate the variable FOO;
  if that is undefined, it will try BAR, and if that too is undefined,
  it will output "BAZ" (that is always defined)


### What is available

- The name of this project, its version, the version of Elixir and OTP
- When the project was built, where was it built and by which user, the build number (if available)
- The current Git SHA that was built, when the last commit was made and by whom.

See https://hexdocs.pm/say_cheez_ex/SayCheezEx.html#info/1 for a full list.

You can also call `SayCheezEx.all()` for a
map with all available attributes:


````
%{
  project_name: "SayCheezEx",
  project_version: "0.1.0-dev",
  build_at: "230213.1617",
  build_at_day: "2023-02-13",
  build_at_full: "2023-02-13.16:17:55",
  build_by: "lenz",
  build_number: "36",
  build_on: "zebra03",
  git_all: "8c0449f/230213.1621",
  git_commit_id: "8c0449f",
  git_commit_id_full: "8c0449fdffc5da6f68237ce8d542ae69ac268cad",
  git_date: "2023-02-13.16:21:12",
  git_date_compact: "230213.1621",
  git_last_committer: "Lenz",
  project_full_version: "0.1.0-dev/8c0449f/230213.1621",
  system: "1.13.4/OTP25",
  system_elixir: "1.13.4",
  system_otp: "25",
  ....
}
````




# Roadmap

- Display runtime information (memory, cpu) in a compact and handy way



