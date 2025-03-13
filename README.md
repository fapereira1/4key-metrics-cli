# 4-Key Metrics CLI

A command-line tool to calculate the Four Key Metrics for software delivery performance as defined in the book "Accelerate: Building and Scaling High Performing Technology Organizations". This tool analyzes Git repositories to provide quantitative measurements of software delivery performance.

## Overview

The tool calculates the following metrics:

1. **Deployment Frequency**
   - Measures how often you deploy code to production
   - Calculated as the average time between deployments

2. **Lead Time for Changes**
   - Measures the time it takes to go from code committed to code running in production
   - Calculated as the average time between commit and deployment dates

3. **Mean Time to Restore (MTTR)**
   - Measures how long it takes to recover from failures in production
   - Calculated using fix deployments identified by special tags

4. **Change Failure Rate**
   - Measures how often deployment failures occur
   - Calculated as the percentage of deployments requiring fixes

## Requirements

- Go 1.24 or higher
- Git repositories with properly tagged releases
- Access to the Git repositories you want to analyze

## Installation

```bash
go install github.com/fapereira1/4key-metrics-cli@latest
```

## Usage

```bash
metrics [options...] [repositories...]
```

### Options

- `-r` Releases tag regex pattern (default: "releases/\\d+")
  - Used to identify regular deployment tags
  - Example matches: releases/1, releases/42

- `-f` Fix tag regex pattern (default: "releases/fix/\\d+")
  - Used to identify fix deployment tags
  - Example matches: releases/fix/1, releases/fix/42

- `-s` Earliest start date of sprint (RFC3339 format)
  - Format: YYYY-MM-DDThh:mm:ss+zz:zz
  - Example: 2020-04-22T16:00:00+03:00

- `-d` Sprint days size (default: 7)
  - Number of days to include in each analysis period

### Example

Analyze two repositories for a specific time period:
```bash
metrics -s 2020-04-22T16:00:00+03:00 ~/code/my-api ~/code/my-api-2
```

Analyze with custom tag patterns:
```bash
metrics -r "v\\d+" -f "hotfix/v\\d+" -s 2020-04-22T16:00:00+03:00 ~/code/my-api
```

## Output

The tool outputs data in CSV format with the following columns:

| Column | Description |
|--------|-------------|
| repository | Name of the analyzed repository |
| from_date | Start date of the analysis period |
| to_date | End date of the analysis period |
| deployment_frequency | Average time between deployments |
| delivery_lead_time | Average time from commit to deployment |
| mean_time_to_restore | Average time to recover from failures |
| change_failure_rate | Percentage of deployments needing fixes |

## Git Requirements

The tool expects your Git repository to have:

1. **Release Tags**: Regular deployment tags matching the release pattern
   - Default pattern: releases/\\d+
   - Example: releases/1, releases/2

2. **Fix Tags**: Tags for fix deployments matching the fix pattern
   - Default pattern: releases/fix/\\d+
   - Example: releases/fix/1, releases/fix/2

Tags must be created with timestamps for accurate metric calculation.

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

### Development Setup

1. Clone the repository
```bash
git clone https://github.com/fapereira1/4key-metrics-cli.git
cd 4key-metrics-cli
```

2. Install dependencies
```bash
go mod tidy
```

3. Build the project
```bash
go build ./...
```

4. Run tests
```bash
go test ./...
```

## License

This project is open source and available under the MIT License.