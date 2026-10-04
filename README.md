# Multi-Table Business Data Analysis & Risk Audit (SQL)

## Project Overview
This repository contains a production-grade relational database schema and 
analytical audit framework designed to evaluate enterprise B2B sales pipelines 
and financial risk metrics. 

## Business Problem Addressed
In corporate transactions, capital retention due to customer billing disputes 
directly harms corporate cash flow. This project builds a data pipeline to 
connect account structures with real-time transactions to calculate contract 
dispute rates across distinct corporate regions.

## Tech Stack
* Database Engine: PostgreSQL 16
* Analytics Tools: Power BI Desktop (Data Modeling & Visualization)
  
## Core Database Schema
* `customers__`: Tracking client demographics, regional classifications, and 
historical signups.
* `transactions__`: Tracking chronological order history, monetary transaction 
values, and invoice settlement statuses.

## Key Insights Delivered
* Isolated high-risk revenue leakage points (e.g., specific corporate accounts 
carrying a ~38% dispute rate).
* Provided regional data feeds to optimize proactive credit line allocation for 
management
