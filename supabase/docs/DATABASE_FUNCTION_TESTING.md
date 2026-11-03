# Database Function Testing

## 1. Overview

Dokumentasi ini berisi hasil pengujian function `check_room_availability()` pada database BanquetBook menggunakan Supabase PostgreSQL.

Pengujian dilakukan untuk memvalidasi logika pengecekan ketersediaan ruangan berdasarkan rentang waktu reservasi dan status reservasi yang tersimpan di database.

**Function:** `banquet_v2.check_room_availability()`

**Testing method:** SQL Function Testing (Read-Only)

## 2. Function Parameters

| Parameter | Data Type | Description |
|---|---|---|
| `p_room_ids` | `bigint[]` | Daftar ID ruangan yang diperiksa |
| `p_starts_at` | `timestamptz` | Waktu mulai reservasi |
| `p_ends_at` | `timestamptz` | Waktu selesai reservasi |

## 3. Test Scenarios and Results

| No. | Test Scenario | Room ID | Expected Result | Actual Result | Status |
|---|---|---:|---|---|---|
| 1 | Existing active reservation overlap | 1 | false | false | PASS |
| 2 | Existing active reservation overlap | 6 | false | false | PASS |
| 3 | Non-overlapping schedule | 1 | true | true | PASS |
| 4 | Non-overlapping schedule | 6 | true | true | PASS |
| 5 | Existing reservation has Expired status | 1 | true | true | PASS |
| 6 | New reservation starts exactly when previous reservation ends | 6 | true | true | PASS |

## 4. Testing Observations

Based on the test results:

1. Reservations with `Pending` status are treated as unavailable when their schedules overlap.
2. Reservations with `Confirmed` status are also treated as unavailable when their schedules overlap.
3. Reservations with `Expired` status are not treated as active schedule conflicts.
4. Two reservations with adjacent time ranges are not considered overlapping.
5. The actual results matched the expected results across all six documented test cases.

## 5. Conclusion

The `check_room_availability()` function passed all six documented test cases.

The test results indicate that the function correctly evaluates room availability based on reservation time overlap and reservation status.

These tests were performed using read-only SQL function calls and did not create or modify booking records.

**Testing scope limitation:** This document covers the tested scenarios only. It does not represent a complete end-to-end booking workflow test.