# Decision Support System for Social Assistance (BPNT) Beneficiary Selection Using SMARTER & ROC Methods

> **Web-Based Decision Support System & Multi-Attribute Beneficiary Ranking Powered by Laravel 12**

![PHP](https://img.shields.io/badge/PHP-%3E%3D%208.2-777BB4?style=for-the-badge&logo=php&logoColor=white)
![Framework](https://img.shields.io/badge/Framework-Laravel%2012-FF2D20?style=for-the-badge&logo=laravel&logoColor=white)
![Database](https://img.shields.io/badge/Database-MySQL-4479A1?style=for-the-badge&logo=mysql&logoColor=white)
![Tailwind CSS](https://img.shields.io/badge/Tailwind_CSS-38B2AC?style=for-the-badge&logo=tailwind-css&logoColor=white)
![Vite](https://img.shields.io/badge/Vite-646CFF?style=for-the-badge&logo=vite&logoColor=white)
![Alpine.js](https://img.shields.io/badge/Alpine.js-8BC0D0?style=for-the-badge&logo=alpine.js&logoColor=white)

---

## Overview & Case Study

The distribution of social welfare programs, such as the Non-Cash Food Assistance program (BPNT - Bantuan Pangan Non Tunai), frequently encounters significant operational challenges. Chief among these are targeting errors (inclusion and exclusion errors), evaluator subjectivity during field verification, and a lack of public transparency regarding selection criteria. 

To overcome these challenges, this Decision Support System (DSS) was designed to establish an objective, verifiable, and merit-based ranking mechanism for prospective aid recipients. The system models multi-attribute socioeconomic conditions through the SMARTER (Simple Multi-Attribute Rating Technique Exploiting Ranks) method, incorporating Rank Order Centroid (ROC) weighting. By structuring qualitative evaluations into rigorous mathematical computations, the platform ensures that aid allocation strictly reflects the degree of recipient vulnerability.

---

## Practical Example

### 1. Criteria Hierarchy & ROC Weighting
Decision criteria are ranked by priority rather than arbitrary manual weights. For $K$ criteria, the weight of the $k$-th ranked criterion ($W_k$) is determined by:

$$W_k = \frac{1}{K} \sum_{i=k}^{K} \frac{1}{i}$$

Consider a scenario with 3 evaluation criteria ordered by priority:
- C1: Household Income (Cost - Rank 1)
- C2: Number of Dependents (Benefit - Rank 2)
- C3: Housing Condition (Cost - Rank 3)

Using the ROC formula ($K = 3$):
- $W_1 = \frac{1}{3} \times (1 + \frac{1}{2} + \frac{1}{3}) = 0.6111$ (61.11%)
- $W_2 = \frac{1}{3} \times (\frac{1}{2} + \frac{1}{3}) = 0.2778$ (27.78%)
- $W_3 = \frac{1}{3} \times (\frac{1}{3}) = 0.1111$ (11.11%)

### 2. Candidate Evaluation & Utility Calculation
Each candidate is scored across sub-criteria. Utility values $u(a_i)$ are scaled from 0 to 1 based on criterion type:
- Benefit criteria: $u(a_i) = \frac{C_{actual} - C_{min}}{C_{max} - C_{min}}$
- Cost criteria: $u(a_i) = \frac{C_{max} - C_{actual}}{C_{max} - C_{min}}$

### 3. Final Preference Value & Ranking
The total preference score $V(A_i)$ aggregates each utility value multiplied by its criterion weight:

$$V(A_i) = \sum_{k=1}^{K} W_k \cdot u_k(A_i)$$

| Candidate | Income Utility ($u_1$) | Dependents Utility ($u_2$) | Housing Utility ($u_3$) | Final Score ($V$) | Rank |
| :--- | :---: | :---: | :---: | :---: | :---: |
| Candidate A | 1.00 | 0.75 | 0.80 | $(0.6111 \times 1.00) + (0.2778 \times 0.75) + (0.1111 \times 0.80) = \mathbf{0.9083}$ | 1 |
| Candidate B | 0.50 | 1.00 | 0.60 | $(0.6111 \times 0.50) + (0.2778 \times 1.00) + (0.1111 \times 0.60) = \mathbf{0.7500}$ | 2 |

Candidate A achieves a higher final preference score and is prioritized as the primary recipient of the assistance.

---

## Technology Stack

- **Backend Framework:** Laravel 12 (PHP 8.2+)
- **Authentication:** Laravel Breeze
- **Database Engine:** MySQL / MariaDB (via Eloquent ORM)
- **Frontend & Styling:** Blade Components, Tailwind CSS, DaisyUI, Alpine.js
- **Asset Bundling:** Vite
- **Reporting & Data Processing:** Barryvdh DomPDF (PDF generation), Maatwebsite Excel (Spreadsheet import/export)
