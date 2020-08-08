<dl>
  <p align="center">
    <img width="320px" src="https://raw.githubusercontent.com/serain/bbrecon/master/docs/logo_cropped.png">
  </p>
  <br />
</dl>

Bug Bounty Recon (`bbrecon`) is Recon-as-a-Service for bug bounty hunters and security researchers. The API aims to provide a continuously up-to-date map of the "safe harbor" attack surface of the Internet, excluding out-of-scope targets.

The service is free.

This repository holds the CLI and Python library. Please see [the website](https://bugbountyrecon.com/) for more details.

## Important Notice

While effort is taken to ensure the results returned by `bbrecon` are reliable and trustworthy, this service and its operators are in no way responsible for what you do with the data provided.

Double check your scopes and ensure you stay within safe harbors.

## Features

- **Public Programs** - public bug bounty programs indexed and searchable with filters (live)
- **Domains** - all domains in scope across all programs (August 2020)
- **Private Programs** - support for private programs (September 2020)
- **Notifications** - webhook alerts when programs are created, updated or domains discovered (September 2020)
- **Endpoints** - all HTTP and non-HTTP endpoints in scope across all programs (October 2020)

## Getting Started

### API key

Fetch an API key from the Console: https://console.bugbountyrecon.com

Only Google SSO is supported at this time.

### Installation

```
$ pip3 install bbrecon

$ bbrecon configure key
Enter your API key: YOUR_API_KEY
```

## Usage

The following will output all programs released in the last month that have "web" type targets (APIs/web apps):

```
$ bbrecon get programs --type web --since last-month
SLUG                  PLATFORM     CREATED     REWARDS      MIN.BOUNTY    AVG.BOUNTY    MAX.BOUNTY      SCOPES  TYPES
cybrary               bugcrowd     2020-07-22  fame         $0            $0            $0                   6  android,ios,web
expressvpn            bugcrowd     2020-07-14  cash,fame    $150          $1047         $2500               17  android,ios,other,web
prestashop            yeswehack    2020-07-23  cash         $0            $0            $1000                1  web
...
```

You can get information about specific programs by passing one or many slugs to the `get programs` command:

```
$ bbrecon get programs twago optimizely
SLUG        PLATFORM    CREATED     REWARDS    MIN.BOUNTY    AVG.BOUNTY    MAX.BOUNTY      SCOPES  TYPES
twago       intigriti   2020-04-09             $0            $0            $0                   5  web
optimizely  bugcrowd    2018-03-22  cash,fame  $0            $750          $5000                6  web
```

To get scopes for specific programs, use `get scopes`:

```
$ bbrecon get scopes rockset codefi-bbp
SLUG        PLATFORM    TYPE    VALUE
rockset     hackerone   web     console.rockset.com
rockset     hackerone   web     docs.rockset.com
rockset     hackerone   web     api.rs2.usw2.rockset.com
codefi-bbp  hackerone   web     activate.codefi.network
```

Use `--help` to get a list of filters for each command:

```
$ bbrecon get programs --help
...
                                  Output format.  [default: wide]
  -n, --name TEXT                 Filter by name.
  -t, --type TEXT                 Filter by scope type. Can be used multiple
                                  times.

  -r, --reward TEXT               Filter by reward type. Can be used multiple
                                  times.

  -p, --platform TEXT             Filter by platform. Can be used multiple
                                  times.

  --exclude-platform TEXT         Exclude specific platform. Ignored if
                                  --platform was passed. Can be used multiple
                                  times.

  -s, --since TEXT                Filter by bounties created after a certain
                                  date. A specific date in the format
                                  '%Y-%m-%d' can be supplied. Alternatively,
                                  the following keywords are supported:
                                  'yesterday', 'last-week', 'last-month',
                                  'last-year' as well as 'last-X-days' (where
                                  'X' is an integer).
...
```

Note that some filters are lists, and can be used multiple times! If you wanted to get all programs that have mobile apps in scope you could run:

```
$ bbrecon get programs --type android --type ios
SLUG                                              PLATFORM     CREATED     REWARDS      MIN.BOUNTY    AVG.BOUNTY    MAX.BOUNTY      SCOPES  TYPES
square                                            bugcrowd     2018-03-22  cash,fame    $300          $492          $5000                4  android,ios,other,web
gojek                                             bugcrowd     2018-03-22  cash,fame    $200          $618          $5000                4  android,ios,web
smartthings                                       bugcrowd     2018-03-22  fame         $0            $0            $0                   5  android,hardware,ios,web
...
```

The default output is a `wide` table. If you're working in a small terminal and want a bit less information, you can pass `--output narrow`:

```
$ bbrecon get programs --type web --since last-month --output narrow                                                                                                                                                                  15:08:41
SLUG                  PLATFORM     REWARDS      MAX.BOUNTY    TYPES
cybrary               bugcrowd     fame         $0            android,ios,web
expressvpn            bugcrowd     cash,fame    $2500         android,ios,other,web
prestashop            yeswehack    cash         $1000         web
...
```

All API programs can return results as
