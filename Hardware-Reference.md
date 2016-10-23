# Introduction

This page provides details for hardware developers for future boards to ensure maximum compatibility with Betaflight.

# Target Maintenance

A hardware developer is responsible for developing, and maintaining, their target within Betaflight. Target files are being separated as much as possible to the main code so as to facilitate this.

# Adding new targets

If you are adding a new flight controller then:

1. Make any PRs against the `development` branch not the `master` branch.
2. Don't change the `travis.yml` or `fake_travis_build.sh` files - these are just for a subset off all builds to check PRs
3. Add page to wiki describing the flight controller and giving a link to at least one supplier.


# Hardware

## MPU (SPI versus I2C)

## MPU Interrrupt

## Blackbox Flash

