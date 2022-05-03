# Please

Have you ever forget how to run old projects. Well, Please will like a dark magical
to help you.

You can configure it to match your project and execute the command to run.

## Installation

You can simply copy `please` file and store in executable path. For example
`$HOME/.local/bin`

Remember to grant executable mode by `chmod +x please`

### Dependencies

- Jq

## Usage

This tool is test in Ubuntu and may work in MacOS, if you use Window then please
use this with WSL2.

Firstly, you need to create a `please.json` file to store configuration then all
child directories will recognize it unless there is nearer `please.json` file
in their parent directories.

```json
{
  "projects": [
    {
      "files": [
        {
          "name": "pom.xml",
          "match": "<groupId>org.springframework.boot</groupId>"
        }
      ],
      "run": "mvn spring-boot:run",
      "clean": "mvn clean install",
      "priority": 1
    },

    {
      "directory": "multi-module-example",
      "run": "mvn spring-boot:run -pl main-module-name",
      "clean": "mvn clean install",
      "priority": 2
    },

    {
      "files": [
        {
          "name": "build.gradle",
          "match": "org.springframework.boot"
        }
      ],
      "run": "./gradlew bootRun"
    },

    {
      "directory": "gin-example",
      "files": [{ "name": "go.mod" }],
      "run": "go run main"
    }
  ],

  "run": "echo \"run\"",
  "deploy": "echo \"deploy\""
}
```

After create `please.json` file, you can execute command such as `please run` and
Please will try to match project with exactly command that you configure. More
detail about `please.json` file, you can find in below section.

## Docs

First thing first, Please assumes that there are only 5 type of command
**run, install, build, clean, deploy**. If your project need more, please wait
while I working to implement **do** command that supports sub-commands.

### Run, Install, Build, Clean, Deploy

If your `please.json` file has one of these command then you can use it by execute.

```json
{
  "run": "echo \"Hello world\""
}
```

```bash
please run
```

One thing you should remember, if command in the highest of json file then it
can only execute in the same directory as `please.json`

### Projects

If you want to use it with child directory and even current directory, then you
can use `projects` field.

```json
{
  "projects": [
    {
      "files": [
        {
          "name": "pom.xml",
          "match": "<groupId>org.springframework.boot</groupId>"
        }
      ],
      "run": "mvn spring-boot:run",
      "clean": "mvn clean install",
      "priority": 1
    },

    {
      "directory": "multi-module-example",
      "run": "mvn spring-boot:run -pl main-module-name",
      "clean": "mvn clean install",
      "priority": 2
    },

    {
      "files": [
        {
          "name": "build.gradle",
          "match": "org.springframework.boot"
        }
      ],
      "run": "./gradlew bootRun"
    },

    {
      "directory": "gin-example",
      "files": [{ "name": "go.mod" }],
      "run": "go run main"
    }
  ],

  "run": "echo \"run\"",
  "deploy": "echo \"deploy\""
}
```

Remember you must specific at least `directory` field or `files` so that Please
can match configuration with your projects.

When you specific `directory` field, then that command will only work if your project
has the same name.

When you specific `files` field, you can match projects that have these file names.
Moreover, you can add `match` field to check the framework of project. For
instance.

```json
{
  "projects": [
    {
      "files": [
        {
          "name": "pom.xml",
          "match": "<groupId>org.springframework.boot</groupId>"
        }
      ],
      "run": "mvn spring-boot:run",
      "clean": "mvn clean install",
      "priority": 1
    }
  ]
}
```

This file with match projects that have `pom.xml` file and this file also has
`<groupId>org.springframework.boot</groupId>`. Then **run** command will
execute `mvn spring-boot:run` to run Spring Boot project.

One more thing, sometimes Please may match more than one configuration for your
projects. Therefore, you should specific `priority` which bigger is better. In
rare cases, when there are more than one configuration match, Please will show
an error.

Finally, in the directory of `please.json` file, command in the highest of json
will override all matching configuration in `projects` field.

```json
{
  "projects": [
    {
      "files": [
        {
          "name": "pom.xml",
          "match": "<groupId>org.springframework.boot</groupId>"
        }
      ],
      "run": "echo \"projects match\"",
      "priority": 1
    }
  ]

    "run": "echo \"match\"",
}
```

If you put this file in Spring Boot project and run `please run`. This will
show "match" even `projects` also matches the projects

## TODO

- [ ] Refactor code
- [ ] Support shorthand json syntax
- [ ] Support ripgrep, fd
- [x] Support yaml configuration with yq
- [x] Support specific configuration file
- [ ] Add **info** command to get info
- [ ] Add **generate** command to generate configuration file
- [x] Add **test** command
- [ ] Add **do** command
- [ ] Add **new** command
- [ ] Check dependencies exit
- [ ] Support regex
- [ ] Style help command

## Contributing

Pull requests are welcome. For major changes,
please open an issue first to discuss what you would like to change.

Please make sure to update tests as appropriate.

## License

[MIT](https://choosealicense.com/licenses/mit/)
