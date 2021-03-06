# First order customization

Kinflate offers simple customization directly in the manifest.

This is _first-order_ customization, a simple fork
of `Kube-manifest.yaml`.

## Names

The simplest form of customization is to change
resource names.  In DAM, this is done by adding a
prefix.

Be sure the manifest is in original form:
<!-- @resetManifest @test -->
```
cp $TUT_ORG_MANIFEST $TUT_APP_MANIFEST
```

Add a _namePrefix_ specification to the manifest:
<!-- @addNamePrefix @test -->
```
cd $TUT_APP
kinflate setnameprefix acme-
# echo "namePrefix: acme-" >> $TUT_APP_MANIFEST
```

Run it:

<!-- @runKinflate @test -->
```
kinflate inflate -f $TUT_APP >$TUT_TMP/customized_out
```

Compare to see result:

<!-- @checkDiffs @test -->
```
diff $TUT_TMP/original_out $TUT_TMP/customized_out
```

## Labels and annotations

Just add some new fields directly to the manifest:

<!-- @addLabelsAndAnnotations @test -->
```
cat <<'EOF' >>$TUT_APP_MANIFEST
objectLabels:
  app: acmehello
  org: acme-corporation
objectAnnotations:
  note: Generated by a tutorial.
EOF
```

<!-- @runKinflateAgain @test -->
```
kinflate inflate -f $TUT_APP >$TUT_TMP/customized_out
```

<!-- @checkDiffsAgain @test -->
```
diff $TUT_TMP/original_out $TUT_TMP/customized_out | more
```

At this point, an end user could check the manifest and
its resources into her version control repository.

It becomes her own version of the _app_.

She can capture upgrades (manifest and resource
changes) by rebasing from the original.
