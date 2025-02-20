---
title: "STMPointFromWKB (geography Data Type)"
description: "STMPointFromWKB (geography Data Type)"
author: MladjoA
ms.author: mlandzic
ms.date: "07/30/2017"
ms.service: sql
ms.subservice: t-sql
ms.topic: reference
ms.custom:
  - ignite-2024
f1_keywords:
  - "STMPointFromWKB (geography Data Type)"
  - "STMPointFromWKB_TSQL"
helpviewer_keywords:
  - "STPointFromWKB method"
dev_langs:
  - "TSQL"
---
# STMPointFromWKB (geography Data Type)
[!INCLUDE [SQL Server Azure SQL Database Azure SQL Managed Instance FabricSQLDB](../../includes/applies-to-version/sql-asdb-asdbmi-fabricsqldb.md)]

Returns a **geographyMultiPoint** instance from an Open Geospatial Consortium (OGC) Well-Known Binary (WKB) representation.
  
## Syntax  
  
```  
  
STMPointFromWKB ( 'WKB_multipoint' , SRID )  
```  
  
## Arguments
 *WKB_multipoint*  
 Is the WKB representation of the **geographyMultiPoint** instance you wish to return. *WKB_multipoint* is a **varbinary(max)** expression.  
  
 *SRID*  
 Is an **int** expression representing the spatial reference ID (SRID) of the **geographyMultiPoint** instance you wish to return.  
  
## Return Types  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] return type: **geography**  
  
 CLR return type: **SqlGeography**  
  
 OGC type: **MultiPoint**  
  
## Remarks  
 This method throws a **FormatException** if the input is not well-formatted.  
  
## Examples  
 The following example uses `STMPointFromWKB()` to create a `geography` instance.  
  
```sql
DECLARE @g geography;   
SET @g = geography::STMPointFromWKB(0x0104000000020000000101000000D7A3703D0A975EC08716D9CEF7D347400101000000CBA145B6F3955EC08716D9CEF7D34740, 4326);  
SELECT @g.ToString();  
```  
  
## See Also  
 [OGC Static Geography Methods](../../t-sql/spatial-geography/ogc-static-geography-methods.md)  
  
  
