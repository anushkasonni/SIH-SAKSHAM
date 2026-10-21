# Database Schemas & Sync Contracts

This document establishes the structural definitions for the offline (SQLite) and online (MongoDB) environments. To prevent identity conflicts while offline, all primary keys use client-side generated UUID strings.

## 1. Smart Mobility / Traffic Module
### Table/Collection Name: `traffic_incidents`

| Field Name | Data Type | Description |
| :--- | :--- | :--- |
| `id` | TEXT (UUID) | Primary Key (Generated locally) |
| `citizen_id` | TEXT | Identifier of the reporter |
| `latitude` | REAL | Geographic coordinate |
| `longitude` | REAL | Geographic coordinate |
| `incident_type` | TEXT | e.g., 'construction', 'accident' |
| `status` | TEXT | 'active', 'resolved' |
| `updated_at` | TEXT | Used for Conflict Resolution (LWW) |

## 2. Smart Healthcare Module (ABHA Integrated)
### Table/Collection Name: `patient_logs`

| Field Name | Data Type | Description |
| :--- | :--- | :--- |
| `id` | TEXT (UUID) | Primary Key (Generated locally) |
| `abha_id` | TEXT | Verified Government Health Account ID |
| `symptoms` | TEXT | Text summary of patient condition |
| `facility_id` | TEXT | Target rural hospital ID |
| `updated_at` | TEXT | Used for Conflict Resolution (LWW) |

## 3. Local Staging Pipeline
### Local Table Name: `sync_queue`
```sql
CREATE TABLE sync_queue (
    id TEXT PRIMARY KEY,
    target_table TEXT NOT NULL,
    action_type TEXT NOT NULL,
    payload TEXT NOT NULL,
    timestamp TEXT NOT NULL,
    sync_status TEXT DEFAULT 'pending'
);
```
