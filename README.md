# Raft3D - Distributed 3D Prints Management using Raft Consensus Algorithm

Raft3D is a distributed backend system designed for managing 3D print jobs, printers, and filament inventory across a network of nodes using the Raft consensus algorithm. The system ensures fault-tolerant, consistent, and replicated state management without relying on centralized databases.

This project is part of the Cloud Computing curriculum at PES University.

## Key Objectives

- Implement a fault-tolerant 3D printer backend using Raft
- Achieve distributed consistency using leader election and log replication
- Build RESTful APIs for managing printers, filaments, and print jobs
- Demonstrate core Raft features: leader election, log replication, snapshotting, and node failure recovery

## Technologies Used

- Language: Go 
- Raft Library: hashicorp/raft
- REST API: HTTP Server of your chosen language
- Optional: Docker or multi-process setup for simulating nodes

## Core Components

### Raft Node

Each node in the Raft3D cluster should:
- Participate in leader election
- Maintain a replicated event log
- Apply events to a Finite State Machine (FSM)
- Handle failover and snapshotting

### FSM State (Objects Stored)

1. Printers
{
  "id": "printer1",
  "company": "Creality",
  "model": "Ender 3"
}

2. Filaments
{
  "id": "fil1",
  "type": "PLA",
  "color": "blue",
  "total_weight_in_grams": 1000,
  "remaining_weight_in_grams": 1000
}

3. Print Jobs
{
  "id": "job1",
  "printer_id": "printer1",
  "filament_id": "fil1",
  "filepath": "prints/sword/hilt.gcode",
  "print_weight_in_grams": 150,
  "status": "Queued"
}

## API Specification

Endpoint: POST /api/v1/printers  
Description: Create a new printer

Endpoint: GET /api/v1/printers  
Description: List all printers

Endpoint: POST /api/v1/filaments  
Description: Add a new filament roll

Endpoint: GET /api/v1/filaments  
Description: List all filaments

Endpoint: POST /api/v1/print_jobs  
Description: Create a new print job (validates weights, IDs)

Endpoint: GET /api/v1/print_jobs  
Description: List all print jobs (optionally filter by status)

Endpoint: POST /api/v1/print_jobs/<job_id>/status?status=<new_status>  
Description: Update job status (queued → running → done or cancelled)

### Status Rules

- Only valid transitions allowed: Queued → Running → Done
- Done jobs must reduce filament’s remaining weight
- Cancelled jobs don’t alter filament weight

## References

- Raft Visualization: https://thesecretlivesofdata.com/raft/
- Raft Paper & Docs: https://raft.github.io/
- Hashicorp Raft: https://github.com/hashicorp/raft

## License

This project is academic and intended for learning distributed consensus via Raft. Not intended for production use without further enhancements.
