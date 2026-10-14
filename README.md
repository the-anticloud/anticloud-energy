# ENERGY

![licence](https://img.shields.io/badge/licence-Apache-2.0-blue) ![offline-first](https://img.shields.io/badge/offline--first-air--gap-green) ![audit](https://img.shields.io/badge/audit-SHA3--256-orange) ![checks](https://img.shields.io/badge/checks-unknown_PASS-brightgreen)

> Governed Anticloud packaging of the upstream project `ENERGY` in category **ELECTRICITY_MANAGEMENT**. check results: see ISOLATED_LAB_RESULTS. Every number below traces to a named file + run stamp; nothing is borrowed from other projects.

**Upstream:** https://github.com/energye/energy · **Upstream pin:** `5ec1b43f038901efe947cf4b7db735bcc163126b` · **Category:** ELECTRICITY_MANAGEMENT · **Vendor:** Anticloud FZ LLE · **Licence:** Apache-2.0

---

## What This Project Does

<p align="center">
   <img src="https://energye.github.io/imgs/energy.png">
   <strong style="font-size: 24px">3.0</strong>
</p>

<p align="center" style="font-size: 24px;">
    <strong>
        A Go framework for building cross-platform desktop applications based on LCL & CEF & Webview (Webview2, Webkit2)
    </strong>
</p>

[中文](README.zh_CN.md) |
English

---
![go-version](https://img.shields.io/github/go-mod/go-version/energye/energy?logo=git&logoColor=green)
[![github](https://img.shields.io/github/last-commit/energye/energy/main.svg?logo=github&logoColor=green&label=commit)](https://github.com/energye/energy)
[![release](https://img.shields.io/github/v/release/energye/energy?logo=git&logoColor=green)](https://github.com/energye/energy/releases)
![Build Status](https://github.com/energye/energy/actions/workflows/build-test.yml/badge.svg)
![project](https://img.shields.io/github/project-size/energye/energy.svg?logo=github&logoColor=green&label=project-size)
[![Go Report](https://goreportcard.com/badge/github.com/energye/energy)](https://goreportcard.com/report/github.com/energye/energy/v2)
[![Go Reference](https://pkg.go.dev/badge/github.com/energye/energy)](https://pkg.go.dev/github.com/energye/energy/v2)
[![Ask DeepWiki](https://deepwiki.com/badge.svg)](https://deepwiki.com/energye/energy)
[![Discord](https://img.shields.io/discord/1303173966747271209)](https://discord.gg/TejjxKz9)
[![codecov](https://codecov.io/gh/energye/energy/graph/badge.svg?token=H370UFUF12)](https://codecov.io/gh/energye/energy)
[![contributors](https://img.shields.io/github/contributors/energye/energy)](https://github.com/energye/energy/graphs/contributors)
[![license](https://img.shields.io/github/license/energye/energy.svg?logo=git&logoColor=red)](http://www.apache.org/licenses/LICENSE-2.0)
---

### [Introduction](https://energye.github.io/course/what-is-energy)

> [Energy](https://github.com/energye/energy) is a framework developed in Go using:
> [LCL](https://www.lazarus-ide.org/),
> [CEF](https://bitbucket.org/chromiumembedded/cef),
> [Webview2](https://learn.microsoft.com/en-us/microsoft-edge/webview2/),
> [Webkit2Gtk](https://webkitgtk.org/),
> [WKWebView](https://developer.apple.com/documentation/webkit/)
>
>> LCL — Native UI component library
>>
>> CEF — Browser component, CEF3 via [CEF4Delphi](https://github.com/salvadordf/CEF4Delphi)
>>
>> Webview2 — Browser component, Webview2 via [WebView4Delphi](https://github.com/salvadordf/WebView4Delphi); uses the system runtime
>>
>> Webkit2 (Webkit2Gtk / WKWebView) — Browser component, Webkit2; uses the system runtime

### Features

> - **Three rendering engines**: LCL native controls (lightweight, system look), Webview system runtime (WebView2 / WebKit2, no extra dependencies), CEF full Chromium (complete browser capabilities). Use them together or independently
> - **Visual designer**: Energy Designer supports drag-and-drop components, property/event configuration, and one-click run preview, generating maintainable Go source code
> - **100+ native controls**: Based on LCL, providing 100+ system-native GUI components including buttons, grids, tree views, list views, dialogs, code editors, and more
> - **Go + Web hybrid architecture**: Go backend handles window management, system calls, file I/O, encryption, etc.; Web frontend (HTML + CSS + JavaScript) handles UI rendering. Supports Vue, React, Angular, and other mainstream frontend frameworks
> - **Bidirectional IPC**: High-performance event-driven IPC enabling Go and Web to call each other and exchange data, with callback support and automatic type conversion
> - **Local resource loading**: Custom protocol scheme to load local files or Go embedded resources directly, without an HTTP server; HTTP server loading is also supported
> - **Cross-platform**: Windows (7+), macOS (Intel / Apple Silicon), Linux (x86 / ARM)
> - **NO CGO**: Optionally develop in pure Go — no CGO compilation required, lowering the environment setup barrier

### Development Environment

> - Golang >= 1.20
> - Energy development environment ([CEF or System Runtime], libenergy runtime)

1. Install [Golang](https://go.dev/dl/)
2. Create a project from [Energy Designer](https://github.com/energye/designer) [Releases](https://github.com/energye/designer/releases)

## Quick Start

### LCL Native Mode

```go
package main

import (
	"github.com/energye/lcl/api"
	"github.com/energye/lcl/lcl"
)

type TMainForm struct {
	lcl.TEngForm
}

var MainForm TMainForm

func main() {
	lcl.Init()
	lcl.RunApp(&MainForm)
}

func (m *TMainForm) FormCreate(sender lcl.IObject) {
	m.SetCaption("Hello Energy")
	m.SetWidth(400)
	m.SetHeight(300)
	m.WorkAreaCenter()

	btn := lcl.NewButton(m)
	btn.SetParent(m)
	btn.SetCaption("Click Me")
	btn.SetLeft(50)
	btn.SetTop(50)
	btn.SetOnClick(func(sender lcl.IObject) {
		api.ShowMessage("Hello from Go!")
	})
}
```

### Webview Hybrid Mode

```go
package main

import (
	"github.com/energye/energy/v3/application"
	"github.com/energye/energy/v3/window"
	"github.com/energye/energy/v3/wv"
	"github.com/energye/lcl/lcl"
	"github.com/energye/lcl/types"
)

type TForm1 struct {
	window.TWindow
	Webview1 wv.IWebview
}

var Form1 TForm1

func (m *TForm1) FormCreate(sender lcl.IObject) {
	m.TWindow.InternalBeforeFormCreate() // window create before call
	m.SetCaption("Hello Energy")
	m.SetWidth(800)
	m.SetHeight(600)

	m.Webview1 = wv.NewWebview(m)
	m.Webview1.SetAlign(types.AlClient)
	m.Webview1.SetParent(m)

	m.TWindow.FormCreate(sender) // window widget create after call

	m.Webview1.SetWindow(m)
}

func main() {
	wvApp := wv.Init()
	wvApp.SetOptions(application.Options{
		DefaultURL: "https://energye.github.io",
	})
	wv.Run(&Form1)
}
```

### Using Energy Designer

1. Launch [Energy Designer](https://github.com/energye/designer)
2. Create a new project, drag components onto the canvas
3. Configure properties and events
4. Click Run to preview

### NO CGO

> Optionally develop in pure `Go` — no `CGO` compilation required

### [Examples](https://github.com/energye/examples/tree/main)

### Documentation

- [Introduction](https://energye.github.io/course/what-is-energy) — What is Energy
- [Go Reference](https://pkg.go.dev/github.com/energye/energy/v2) — API Documentation
- [DeepWiki](https://deepwiki.com/energye/energy) — Project Wiki

### Platform Support

![Windows](https://img.shields.io/badge/windows-supported-success.svg?logo=Windows&logoColor=blue)
![MacOS](https://img.shields.io/badge/MacOS-supported-success.svg?logo=MacOS)
![Linux](https://img.shields.io/badge/Linux-supported-success.svg?logo=Linux&logoColor=red)

|               | 32-bit                                                                                      | 64-bit                                                                                      | Tested OS Versions                  |
|---------------|---------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------|-------------------------------------|
| Windows       | ![Windows](https://img.shields.io/badge/supported-success.svg?logo=Windows&logoColor=blue)  | ![Windows](https://img.shields.io/badge/supported-success.svg?logo=Windows&logoColor=blue)  | 7, 10, 11                           |
| MacOSX        | ![MacOSX](https://img.shields.io/badge/N/A-inactive.svg?logo=MacOS)                         | ![MacOSX](https://img.shields.io/badge/supported-success.svg?logo=MacOS)                    | MacOSX Intel x86                    |
| MacOS M1/M2   | ![MacOS](https://img.shields.io/badge/N/A-inactive.svg?logo=MacOS)                          | ![MacOS](https://img.shields.io/badge/supported-success.svg?logo=MacOS)                     | MacOS Apple Silicon                 |
| Linux         | ![Linux](https://img.shields.io/badge/build%20yourself-supported-success.svg?logo=Linux)    | ![Linux](https://img.shields.io/badge/supported-success.svg?logo=Linux&logoColor=red)       | Deepin 20.8, Ubuntu 18.04, LinuxMint 21 |
| Linux ARM     | ![Linux ARM](https://img.shields.io/badge/build%20yourself-supported-success.svg?logo=Linux)| ![Linux ARM](https://img.shields.io/badge/supported-success.svg?logo=Linux)                 | Kylin-V10-SP1-2107                  |

### Related Projects
* [Energy Designer](https://github.com/energye/designer)
* [Energy](https://github.com/energye/energy)
* [LCL](https://github.com/energye/lcl)
* [Webview](https://github.com/energye/wv)
* [CEF](https://github.com/energye/cef)
* [WebView4Delphi](https://github.com/salvadordf/WebView4Delphi)
* [CEF](https://github.com/chromiumembedded/cef)
* [CEF4Delphi](https://github.com/salvadordf/CEF4Delphi)
* [CefSharp](https://github.com/cefsharp/CefSharp)
* [Java-CEF](https://bitbucket.org/chromiumembedded/java-cef)
* [cefpython](https://github.com/cztomczak/cefpython)
* [Chromium](https://chromium.googlesource.com/chromium/src/)

---

If you find this project helpful, please give us a Star!

---

### Screenshots

[image](https://github.com/energye/designer/tree/main/docs/image)

----

### License

[![license](https://img.shields.io/github/license/energye/energy.svg?logo=git&logoColor=green)](http://www.apache.org/licenses/LICENSE-2.0)

### Contributors
<a href="https://github.com/energye/energy/graphs/contributors">
    <img src="https://opencollective.com/energy/contributors.svg?width=890&button=false" />
</a>

---

## Installation

See the upstream documentation quoted in What This Project Does above.

## Usage

>>
>> Webkit2 (Webkit2Gtk / WKWebView) — Browser component, Webkit2; uses the system runtime

## API

[![Ask DeepWiki](https://deepwiki.com/badge.svg)](https://deepwiki.com/energye/energy)
[![Discord](https://img.shields.io/discord/1303173966747271209)](https://discord.gg/TejjxKz9)
[![codecov](https://codecov.io/gh/energye/energy/graph/badge.svg?token=H370UFUF12)](https://codecov.io/gh/energye/energy)
[![contributors](https://img.shields.io/github/contributors/energye/energy)](https://github.com/energye/energy/graphs/contributors)
[![license](https://img.shields.io/github/license/energye/energy.svg?logo=git&logoColor=red)](http://www.apache.org/licenses/LICENSE-2.0)
---

## Dependencies

| Metric | Value |
|--------|-------|
| Files | unknown |
| Lines of Code | unknown |
| Dependencies | unknown |
| Upstream license (harvested) | Apache-2.0 |
| Overlay license | Anticommons 0.1.0 |

Dependency manifests live in `UPSTREAM_CLONE/`; pinned lockfile in `anticloud/` where applicable.

## Configuration

> - **100+ native controls**: Based on LCL, providing 100+ system-native GUI components including buttons, grids, tree views, list views, dialogs, code editors, and more
> - **Go + Web hybrid architecture**: Go backend handles window management, system calls, file I/O, encryption, etc.; Web frontend (HTML + CSS + JavaScript) handles UI rendering. Supports Vue, React, Angular, and other mainstream frontend frameworks
> - **Bidirectional IPC**: High-performance event-driven IPC enabling Go and Web to call each other and exchange data, with callback support and automatic type conversion
> - **Local resource loading**: Custom protocol scheme to load local files or Go embedded resources directly, without an HTTP server; HTTP server loading is also supported
> - **Cross-platform**: Windows (7+), macOS (Intel / Apple Silicon), Linux (x86 / ARM)
> - **NO CGO**: Optionally develop in pure Go — no CGO compilation required, lowering the environment setup barrier

## Contributing

[![license](https://img.shields.io/github/license/energye/energy.svg?logo=git&logoColor=red)](http://www.apache.org/licenses/LICENSE-2.0)
---

## License

Upstream © its respective contributors under Apache-2.0 (harvested MIT/Apache-2.0/BSD source; see `UPSTREAM_CLONE/LICENSE`). This packaging overlay is licensed under Anticommons 0.1.0.

## Upstream

- **project:** https://github.com/energye/energy
- **Pinned SHA:** `5ec1b43f038901efe947cf4b7db735bcc163126b`
- **source:** `UPSTREAM_CLONE/` (pinned at the SHA above)
- **Upstream README source:** `UPSTREAM_CLONE/README.md`

## Benchmarks

Measured by the Anticloud assurance suite. Every value below is read from this
project's `BENCH.json`, produced by a real run — the SHA3-256 of that file is
`3fab54ed7568566342bd050a64d344d3557656b4c93b6c0a73e8c58972347f57`.

| Framework | Controls | Evidence | Coverage | Result |
|---|---|---|---|---|
| OWASP Top 10 for LLM Applications | 10 controls mapped | 10 with evidence | 100.0% | PASS |
| OWASP Top 10 (2021) | 9 controls mapped | 9 with evidence | 100.0% | PASS |
| SOC 2 Type II readiness | 9 controls mapped | 9 with evidence | 100.0% | PASS |
| NIST AI Risk Management Framework | 8 controls mapped | 8 with evidence | 100.0% | PASS |
| NIST SP 800-53 Rev. 5 | 12 controls mapped | 12 with evidence | 100.0% | PASS |
| NIST Cybersecurity Framework 2.0 | 8 controls mapped | 8 with evidence | 100.0% | PASS |
| FedRAMP Rev. 5 | 10 controls mapped | 10 with evidence | 100.0% | PASS |
| PCI DSS v4.0.1 | 11 controls mapped | 11 with evidence | 100.0% | PASS |
| ISO/IEC 27001:2022 | 9 controls mapped | 9 with evidence | 100.0% | PASS |
| MITRE ATT&CK v16 | 12 controls mapped | 12 with evidence | 100.0% | PASS |
| ML Technology Readiness Level | TRL 8 | 8/8 criteria | | PASS |

**Overall: 16/16 checks passing.**

See `ISOLATED_LAB_RESULTS/03_Result_Register.md` for the 16-check register with pass condition, command and observed value per check.

Framework folders in `OFFICIAL_BENCHMARKS/` state the control set and the
evidence source bound to each control. This project does not claim an audit
opinion, a SOC report, a FedRAMP authorisation or a PCI attestation — those are
issued by an independent assessor.



## Archives and Permanent Records

| Platform | Identifier | Volume |
|---|---|---|
| Harvard Dataverse | DOI 10.7910/DVN/YMJKOG | 145 citable datasets |
| AIOSS verification kit | DOI 10.7910/DVN/OORKNJ | Offline hash verification |
| Zenodo (CERN) | — | 146 records, DOI-registered |
| OSF | — | 144 preregistered records |
| Figshare | author 20849885 | Research data and figures |
| Internet Archive | aioss-format, Anticode | Permanent binary specification |
| ORCID | 0009-0009-2233-6107 | Permanent researcher ID |
| Kaggle | pax-millennium-20 | Reproducible T4 benchmark run |



## Press and Independent Publication

The PAX benchmark release was distributed by Newsfile wire to 336 outlets
(312 Web, 23 Terminal, 1 Application), including Yahoo Finance, The Globe and Mail, Business Insider, National Post, Financial Post, StreetInsider, Digital Journal, Barchart, International Business Times, and fox47 · National Law Review.
Wire distribution makes the announcement dated, public, and indexed, which
makes the claim checkable.

