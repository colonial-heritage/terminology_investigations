SELECT DISTINCT
        xr.TermID AS LinkedTermID,
        t1.TermMasterID,
        t2.TermID AS TermID,

		t1.Term,
        t2.Term,
        TermTypes.TermType,
		ty.ThesXrefType

  FROM 
  (
        (
                (
                        ThesXrefs xr
                        LEFT JOIN ThesXrefTypes ty
                        ON xr.ThesXrefTypeID = ty.ThesXrefTypeID
                )
                LEFT JOIN Terms t1
                ON xr.TermID = t1.TermID
        )
        LEFT JOIN 
        (
                Terms t2
                LEFT JOIN TermTypes
                ON t2.TermTypeID = TermTypes.TermTypeID
        )
        ON t1.TermMasterID = t2.TermMasterID
)
  WHERE
(
  lower(t1.Term) LIKE '%sirih%'
  OR lower(t1.Term) LIKE '%sireh%'
  OR lower(t1.Term) LIKE '%sereh%'
  OR lower(t1.Term) LIKE '%pinang%'
  OR lower(t1.Term) LIKE '%penang%'
  OR lower(t1.Term) LIKE '%betel%'
)
AND
(
	(ThesXrefType IN ('OBJECTTREFWOORD', 'Voorstelling Object', 'FUNCTIE EN CONTEXT'))
	OR (lower(ThesXrefType) LIKE '%funct%')
	OR (lower(ThesXrefType) LIKE '%materiaal%')
)

ORDER BY TermMasterID, t1.Term, ty.ThesXrefType
