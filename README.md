# BanquetBook — Hotel Banquet Booking System

A web-based banquet booking management system designed to simulate hotel banquet reservation operations.

## Project Overview

BanquetBook is a portfolio project developed to explore how hotel banquet reservation processes can be supported by a structured database system.

The project focuses on room reservations, event packages, additional services, booking validation, and pricing calculations.

## Business Context

Banquet operations involve several components that must be coordinated, including:

- Event and booking information
- Banquet room availability
- Meeting packages
- Additional services
- Reservation schedules
- Booking price calculations

BanquetBook aims to organize these components through an integrated application and relational database.

## Key Features

- Banquet room selection
- Booking date and guest capacity management
- Package selection
- Additional service selection
- Room availability validation
- Booking price calculation
- Reservation submission and database storage

## Technology Stack

- Frontend: Next.js, React, TypeScript
- Backend / Database Platform: Supabase
- Database: PostgreSQL
- Query Language: SQL
- Version Control: Git

## Database Focus

This project explores several database concepts:

- Relational database design
- Data validation
- Booking transaction management
- Room reservation records
- Pricing calculation
- Database functions and stored procedures
- Reservation data snapshots

## Project Status

The application and database logic are under development and testing.

Pricing calculations have been compared between the frontend and an independent SQL simulation for a test scenario. Further end-to-end testing of the booking submission process is still required.

## Project Structure

```text
Banquet-Booking/
├── public/
├── src/
├── supabase/
├── .gitignore
├── package.json
├── package-lock.json
├── next.config.mjs
└── README.md
```

## Learning Objectives

This project is part of a portfolio focused on database development and data engineering, with practical exploration of hospitality business processes.

It demonstrates the application of database concepts to a real-world-inspired hotel operational scenario.