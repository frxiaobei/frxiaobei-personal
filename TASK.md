# Task: Fix Background Color

## Problem
The website https://frxiaobei.com still shows a pinkish/cream background color instead of pure white.

## Current State
- `assets/css/extended/custom.css` has `--cream: #FFFFFF` but it's not being applied
- The background appears pinkish/salmon colored

## Your Task
1. Find ALL places where background color is set (check PaperMod theme files, hugo.toml, any CSS)
2. Ensure the main background is pure white `#FFFFFF` in light mode
3. Check if there's a gradient or other effect causing the pink tint
4. Fix it so the background is clean white
5. Commit your changes with message "fix: ensure pure white background"

## Files to Check
- assets/css/extended/custom.css
- themes/PaperMod/assets/css/
- hugo.toml (profileMode settings)
- layouts/

## Constraints
- Keep dark mode as is (#1a1a1a)
- Don't break other styling
