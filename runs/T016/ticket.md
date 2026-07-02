# T016 — T016 - Add frontend API client and task data hooks

**Source**: GitHub Issue #17

## Description

## Goal

Add the frontend data access layer for tasks.

## Depends on

- T012 - Bootstrap frontend React Vite foundation
- T015 - Implement task CRUD API

## Scope

- Add typed API client for task endpoints.
- Add task fetching and mutation hooks/services.
- Add loading and error handling conventions.
- Add tests for the client/hook layer.

## Acceptance criteria

- Frontend can list, create, update, and delete tasks through the API client.
- API errors are surfaced cleanly.
- No complete task UI page is implemented yet.
