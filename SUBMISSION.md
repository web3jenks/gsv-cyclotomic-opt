# Submission Documentation

This document tells the Kriterion review panel where to find the optimizations for the Groth16 garbled circuit verifier.

## Changes

The optimization improves the hard part of the final exponentiation in `src/gadgets/bn254/final_exponentiation.rs`.

1.  **Cyclotomic Inverse**: We replaced the full $F_{p^{12}}$ inversion in `cyclotomic_exp_fast_inverse_montgomery_fast` with a complex conjugate (`Fq12::conjugate`). The element $f$ is in the cyclotomic subgroup, so $x^{-1} = x^{p^6} = \bar{x}$. This conjugate costs 0 non-free gates.
2.  **Cyclotomic Squaring**: We replaced general squaring (`Fq12::square_montgomery`) with cyclotomic squaring (`Fq12::cyclotomic_square_montgomery`) for $y_1, y_2,$ and $y_5$ in `final_exponentiation_montgomery`.

These changes keep the mathematical logic correct and reduce the number of non-free gates.
