mv ~/Downloads/README.md .

mkdir -p lib tests/fixtures exports
touch lib/.gitkeep tests/fixtures/.gitkeep exports/.gitkeep

cat > .gitignore <<'EOF'
.env
exports/*
!exports/.gitkeep
.DS_Store
EOF

cat > .env.example <<'EOF'
LEAGUE_ID=
SEASON=2026
ESPN_S2=
SWID=
EOF

cat > ff <<'EOF'
#!/usr/bin/env bash
set -euo pipefail

usage() {
  cat <<USAGE
Usage: ff <command> [options]

Commands:
  roster        Show your roster
  week N        Show your week N matchup
  bench         Points left on the bench each week
  waivers       Free agents scoring above projections

Options:
  --csv         Export results to exports/
  -h, --help    Show this help message
USAGE
}

case "${1:-}" in
  -h|--help|"") usage ;;
  *) echo "ff: '$1' is not implemented yet" >&2; exit 1 ;;
esac
EOF
chmod +x ff

./ff --help
