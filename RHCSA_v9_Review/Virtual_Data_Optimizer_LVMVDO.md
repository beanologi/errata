# Errata: Virtual Data Optimizer (LVMVDO)

## Naming Convention Issue

The video unintentionally demonstrates that using hyphen characters to name the volume group as `vdo-vg` results in the output of `lsblk` being cluttered with an excessive number of hyphens. This is due to the way `lsblk` internally uses hyphens as delimiters.

Overall, this is poor practice as it makes the `lsblk` command output unintuitive to read (for humans).

The presenter doesn't directly address the suboptiminal choice in the video so it is clearly in the red zone!

### The Fix:

Use underscores instead of hyphens for naming volume groups.