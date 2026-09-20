# Example: repeat a façade bay

Request: “Use this bay along the east façade, then test a deeper fin.”

This is a decision example, not an executable recipe or fixed call sequence.

1. Identify the source bay and the target façade scope. Establish its local axes and whether the
   designer supplied positions or wants a generated spacing pattern.
2. Check ownership. A nested option is useful if the first bay is to be designed in place; an
   existing saved component can be referenced directly. Follow the current guide's save prerequisites.
3. Keep the bay geometry in its source option. Put the placement pattern in the assembly option.
   Use a recipe only when repeatable generation is wanted. A one-time layout can remain one-time.
4. Save the reviewed source and place its exact snapshot at stable keyed positions. A repeated
   snapshot preserves source history; a copied editable object has different ownership.
5. Test deeper fins in a source alternative. Read affected clearances using the relevant solid or
   surface geometry and spacing using explicit reference geometry. A centreline gap does not prove
   a clear opening between fins.
6. Report the visual and geometric differences. Change assembly pins only if that revision is
   selected within the user's task. The existing assembly snapshot remains reproducible.

If the bay uses an external floor height, record the pinned input. Loading an old bay gives its
sealed geometry; it does not recut it to the newest height. If the façade datum changed, inspect
whether placement is live-aligned or independent before proposing an update.
