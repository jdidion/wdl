# Differences Between WDL Versions 

⚙️denotes a change that affects the parser

## Draft-2 to 1.0

*   ⚙️Version statement required (as of draft-3)
*   ⚙️Structs
*   ⚙️For both tasks and workflows, unbound inputs must be within the `input {}` block
    *   Declarations outside input block cannot be overridden
    *   As a consequence of this, the way workflow inputs are calculated differs between the two specs - refer to the “Computing Inputs” sections for details
*   The workflow `output` section is optional in both specs, but the 1.0 spec clarifies that the output section is required in a workflow called as a sub-workflow.
*   Task command section
    *   ⚙️Placeholders can begin with `~` rather than `$`
    *   ⚙️Placeholders in heredoc-style command (`command <<< >>>`) may only use the `~{}` style placeholder
    *   `command <<< >>>` with `~{}` placeholders is strongly preferred to avoid any mis-parsing of bash variables
    *   In Draft-2 it was allowed to specify only one of the `true/false` options in a placeholder - the other option evaluated to empty string. In 1.0, both options are required.
*   `meta` and `parameter_meta` are allowed to contain number, boolean, object, or array values in addition to strings
*   Standard library
    *   `size` can be computed for certain compound types


## Non-features

These are features that appear in either or both of the Draft-2 and 1.0 specs that are here explicitly specified as non-features _i.e._ consider these sections of the spec(s) as redlined.

*   Both Draft-2 and 1.0 mention a `loop` (or `while`) construct. It appears in both the Draft-2 and 1.0 Hermes grammars. However, I don’t think it was ever supported in Cromwell, and it is dropped in 2.0.
*   ⚙️Wildcard syntax in the output section in Draft-2 is deprecated, and is dropped in 1.0. Parsers are not required to support this syntax in order to be considered compliant.

## Open Questions

These items are ambiguous in either or both of the Draft-2 and 1.0 specs - whether or not they should be considered part of the spec will probably be determined by whether they are implemented in Cromwell.

*   Coercion of Map to Object - 1.0 does support this, but I _think_ Draft-2 does not support this.
*   The Draft-2 spec mentions a `quote` placeholder option once, but it is never referred to again. The Hermes grammar does not enumerate the allowed options. It is definitely not supported in 1.0.
*   The 1.0 spec says that optional inputs with defaults are allowed, whereas the Draft-2 spec does not say one way or the other.
*   The Draft-2 spec says that an optional variable whose value is unspecified will evaluate to the empty string. The 1.0 spec does not say how a variable with an unspecified value is evaluated - I am guessing it is an error?

## References

*   Draft-2
    *   [Specification](https://github.com/openwdl/wdl/blob/master/versions/draft-2/SPEC.md#alternative-heredoc-syntax)
    *   [Hermes grammar](https://github.com/openwdl/wdl/blob/master/versions/draft-2/parsers/grammar.hgr)
*   1.0
    *   [Specification](https://github.com/openwdl/wdl/blob/master/versions/1.0/SPEC.md#command-section)
    *   [Hermes grammar](https://github.com/openwdl/wdl/blob/master/versions/1.0/parsers/hermes/grammar.hgr)
*   [The diff between Draft-2 and 1.0 specs](https://github.com/jdidion/wdl/commit/35b49a815858d45e6111899296ae4beb729fe13a?short_path=22feea2#diff-22feea2e46776b17b2da5ddc2717b767)
