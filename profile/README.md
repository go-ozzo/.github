<h1 align="center">go-ozzo</h1>

<p align="center">
  <strong>Idiomatic Go Libraries for Web Development</strong><br>
  By <a href="https://github.com/qiangxue">Qiang Xue</a>, creator of <a href="https://www.yiiframework.com/">Yii Framework</a>
</p>

<p align="center">
  <a href="https://github.com/go-ozzo/ozzo-validation/issues/207"><img src="https://img.shields.io/badge/Go-1.21+-00ADD8?style=flat&logo=go" alt="Go Version"></a>
  <a href="https://github.com/go-ozzo/ozzo-validation/blob/master/LICENSE"><img src="https://img.shields.io/badge/License-MIT-yellow.svg" alt="License"></a>
  <a href="https://github.com/go-ozzo/ozzo-validation/issues/207"><img src="https://img.shields.io/badge/Status-Actively_Maintained-brightgreen?style=flat" alt="Status"></a>
</p>

---

## Libraries

| | Library | Purpose | Version | Stars | Issues | PRs |
|:-:|:--------|:--------|:-------:|:-----:|:------:|:---:|
| | | ***Data Validation*** | | | | |
| ✅ | **[ozzo-validation](https://github.com/go-ozzo/ozzo-validation)** | Configurable validation using code, not struct tags | [![](https://img.shields.io/github/v/release/go-ozzo/ozzo-validation?style=flat-square&label=)](https://github.com/go-ozzo/ozzo-validation/releases) | [![](https://img.shields.io/github/stars/go-ozzo/ozzo-validation?style=flat-square&label=)](https://github.com/go-ozzo/ozzo-validation/stargazers) | [![](https://img.shields.io/github/issues/go-ozzo/ozzo-validation?style=flat-square&label=)](https://github.com/go-ozzo/ozzo-validation/issues) | [![](https://img.shields.io/github/issues-pr/go-ozzo/ozzo-validation?style=flat-square&label=)](https://github.com/go-ozzo/ozzo-validation/pulls) |
| | | ***HTTP Routing*** | | | | |
| 🌐 | **[ozzo-routing](https://github.com/go-ozzo/ozzo-routing)** | Fast HTTP router with regex matching and RESTful API support | [![](https://img.shields.io/github/v/release/go-ozzo/ozzo-routing?style=flat-square&label=)](https://github.com/go-ozzo/ozzo-routing/releases) | [![](https://img.shields.io/github/stars/go-ozzo/ozzo-routing?style=flat-square&label=)](https://github.com/go-ozzo/ozzo-routing/stargazers) | [![](https://img.shields.io/github/issues/go-ozzo/ozzo-routing?style=flat-square&label=)](https://github.com/go-ozzo/ozzo-routing/issues) | [![](https://img.shields.io/github/issues-pr/go-ozzo/ozzo-routing?style=flat-square&label=)](https://github.com/go-ozzo/ozzo-routing/pulls) |
| | | ***Database*** | | | | |
| 🗄️ | **[ozzo-dbx](https://github.com/go-ozzo/ozzo-dbx)** | DB-agnostic query builder enhancing database/sql | [![](https://img.shields.io/github/v/release/go-ozzo/ozzo-dbx?style=flat-square&label=)](https://github.com/go-ozzo/ozzo-dbx/releases) | [![](https://img.shields.io/github/stars/go-ozzo/ozzo-dbx?style=flat-square&label=)](https://github.com/go-ozzo/ozzo-dbx/stargazers) | [![](https://img.shields.io/github/issues/go-ozzo/ozzo-dbx?style=flat-square&label=)](https://github.com/go-ozzo/ozzo-dbx/issues) | [![](https://img.shields.io/github/issues-pr/go-ozzo/ozzo-dbx?style=flat-square&label=)](https://github.com/go-ozzo/ozzo-dbx/pulls) |
| | | ***Infrastructure*** | | | | |
| 📝 | **[ozzo-log](https://github.com/go-ozzo/ozzo-log)** | High-performance async logging with severity filtering | — | [![](https://img.shields.io/github/stars/go-ozzo/ozzo-log?style=flat-square&label=)](https://github.com/go-ozzo/ozzo-log/stargazers) | [![](https://img.shields.io/github/issues/go-ozzo/ozzo-log?style=flat-square&label=)](https://github.com/go-ozzo/ozzo-log/issues) | [![](https://img.shields.io/github/issues-pr/go-ozzo/ozzo-log?style=flat-square&label=)](https://github.com/go-ozzo/ozzo-log/pulls) |
| ⚙️ | **[ozzo-config](https://github.com/go-ozzo/ozzo-config)** | Layered config in JSON, YAML, TOML | — | [![](https://img.shields.io/github/stars/go-ozzo/ozzo-config?style=flat-square&label=)](https://github.com/go-ozzo/ozzo-config/stargazers) | [![](https://img.shields.io/github/issues/go-ozzo/ozzo-config?style=flat-square&label=)](https://github.com/go-ozzo/ozzo-config/issues) | [![](https://img.shields.io/github/issues-pr/go-ozzo/ozzo-config?style=flat-square&label=)](https://github.com/go-ozzo/ozzo-config/pulls) |
| 💉 | **[ozzo-di](https://github.com/go-ozzo/ozzo-di)** | Dependency injection container | — | [![](https://img.shields.io/github/stars/go-ozzo/ozzo-di?style=flat-square&label=)](https://github.com/go-ozzo/ozzo-di/stargazers) | [![](https://img.shields.io/github/issues/go-ozzo/ozzo-di?style=flat-square&label=)](https://github.com/go-ozzo/ozzo-di/issues) | [![](https://img.shields.io/github/issues-pr/go-ozzo/ozzo-di?style=flat-square&label=)](https://github.com/go-ozzo/ozzo-di/pulls) |

---

## ozzo-validation

The flagship library. Validation using normal Go code instead of error-prone struct tags:

```go
package main

import (
    "fmt"
    validation "github.com/go-ozzo/ozzo-validation/v4"
    "github.com/go-ozzo/ozzo-validation/v4/is"
)

type User struct {
    Name  string
    Email string
    Age   int
}

func (u User) Validate() error {
    return validation.ValidateStruct(&u,
        validation.Field(&u.Name, validation.Required, validation.Length(3, 50)),
        validation.Field(&u.Email, validation.Required, is.EmailFormat),
        validation.Field(&u.Age, validation.Required, validation.Min(18)),
    )
}

func main() {
    u := User{Name: "Jo", Email: "bad", Age: 10}
    fmt.Println(u.Validate())
    // Output: Age: must be no less than 18; Email: must be a valid email address;
    //         Name: the length must be between 3 and 50.
}
```

**Why ozzo-validation over struct tags?**
- IDE autocomplete and compile-time checks
- Complex conditional rules with `When()`
- Custom validators with `By()`
- Map validation without structs
- Context-aware validation

---

## Status

After being unmaintained since 2020, go-ozzo is **actively maintained** again as of July 2026.

**What's happening:**
- Full review of all open PRs and issues completed
- Bug fixes and community contributions being reviewed and merged
- CI being modernized (GitHub Actions, golangci-lint, codecov)
- New validation rules coming: UUID v7, ULID, and more

All improvements are **additive and backward-compatible** — no import path changes, no migration needed. Minimum Go version is being updated from 1.13 to 1.21+ ([discussion](https://github.com/go-ozzo/ozzo-validation/issues/207)).

See [ozzo-validation#207](https://github.com/go-ozzo/ozzo-validation/issues/207) for the full roadmap and discussion.

---

## Maintainers

| | Name | Role |
|:-:|:-----|:-----|
| <img src="https://github.com/qiangxue.png" width="40"> | **[@qiangxue](https://github.com/qiangxue)** | Creator, Yii Framework author |
| <img src="https://github.com/kolkov.png" width="40"> | **[@kolkov](https://github.com/kolkov)** | Active maintainer, [GoGPU](https://github.com/gogpu) author |

---

## Contributing

We welcome contributions! Start with:
- [`good first issue`](https://github.com/go-ozzo/ozzo-validation/labels/good%20first%20issue) labels for newcomers
- Open issues and PRs on individual repositories
- Join the discussion in [ozzo-validation#207](https://github.com/go-ozzo/ozzo-validation/issues/207)

---

## License

All projects are licensed under the **MIT License**.

---

<p align="center">
  <sub>Idiomatic Go libraries for building better web applications</sub><br>
  <a href="https://github.com/go-ozzo">github.com/go-ozzo</a>
</p>
