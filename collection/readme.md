# Bruno GenUI Collection

A comprehensive Bruno API collection for [mockdata.dev](https://mockdata.dev) - a mock API service for testing and development.

## Overview

This collection provides ready-to-use API requests for various domains including billing, flights, hotels, posts, and songs. Each domain includes endpoints for browsing, filtering, ordering, and lookup operations.

## Getting Started

### Prerequisites

- [Bruno](https://www.usebruno.com/) installed on your system
- Access to the mockdata.dev API (publicly available)

### Installation

1. Clone or download this collection
2. Open Bruno
3. Import the collection by selecting the `opencollection.yml` file or the collection folder

### Environment Setup

The collection includes a `Prod` environment configured with:
- **Base URL**: `https://mockdata.dev`

You can create additional environments (e.g., `Local`) by copying the `Prod.yml` file and modifying the `baseUrl` variable.

## Collection Structure

### 📦 Billing

Complete billing system APIs including:

- **Customers**: Get all customers, filter by email, status, date range
- **Invoices**: Retrieve invoices with filters by customer, status, due date
- **Payments**: Query payments by amount range, customer, payment method, status
- **Subscriptions**: Manage subscriptions filtered by customer, plan, status
- **Lookups**: Reference data for currencies, statuses, payment methods, plans

### ✈️ Flights

Flight booking and search APIs:

- **Shop**: Search flights by origin, destination, departure date with pagination
- **Order**: Manage flight orders, filter by origin, status, date
- **Lookups**: Aircraft types, airlines, airports, fare types

### 🏨 Hotels

Hotel booking and search APIs:

- **Shop**: Search hotels by city, check-in date, guests, rooms with pagination
- **Order**: Manage hotel orders, filter by check-in date, city
- **Lookups**: Amenities, cities, hotel chains, property types, room types

### 📝 Posts

Blog/content management APIs:

- **Posts**: Get all posts, filter by category, tags, status, author, date range
- **Authors**: Retrieve authors with filters by email, date range
- **Comments**: Query comments filtered by post, author, status
- **Lookups**: Categories, tags, post statuses, comment statuses

### 🎵 Songs

Music catalog APIs:

- **Shop**: Browse songs with filters and pagination
- **Order**: Manage song orders
- **Lookups**: Genres, artists, albums, and other reference data

## Features

- ✅ **Comprehensive Coverage**: Multiple domains with full CRUD operations
- ✅ **Filter Examples**: Various filtering patterns (date ranges, status, IDs, etc.)
- ✅ **Combined Filters**: Examples of multi-parameter filtering
- ✅ **Pagination**: Ready-to-use pagination examples
- ✅ **Lookup Data**: Reference endpoints for dropdowns and validation
- ✅ **Well Documented**: Each request includes detailed documentation

## Usage Examples

### Basic Request

Most requests follow a simple pattern:

```
GET {{baseUrl}}/billing/customers
```

### Filtered Requests

Many endpoints support filtering:

```
GET {{baseUrl}}/billing/customers?status=active&email=john@example.com
```

### Pagination

Use `page` and `per_page` parameters:

```
GET {{baseUrl}}/posts?page=1&per_page=20
```

## API Documentation

Each request in the collection includes inline documentation explaining:
- Request parameters
- Response structure
- Status codes and values
- Example responses

## Contributing

Feel free to extend this collection with additional endpoints or domains as needed. The structure follows Bruno's OpenCollection format.

## License

This collection is provided as-is for use with the mockdata.dev API service.

