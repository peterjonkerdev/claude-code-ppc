.PHONY: test lint format typecheck all

test:
	uv run pytest tests/ -v

lint:
	uv run ruff check .

format:
	uv run ruff format .

typecheck:
	uv run mypy . --ignore-missing-imports

all: format lint typecheck test
