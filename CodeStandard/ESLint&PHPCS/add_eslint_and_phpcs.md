# 🚀 SETUP LARAVEL AND VUE PROJECT

**🌐 Step To Step To Setup ESLint And PHPCS 🌐**

1. Install dependencies:

```bash
yarn add -D eslint 
composer require --dev squizlabs/php_codesniffer
```

2. Initialize eslint configuration file:

```bash
npm init @eslint/config@latest
# or
yarn create @eslint/config
# or
pnpm create @eslint/config@latest


```

3. Add this configuration to the script of composer.json file:

```json
"scripts": {
  "cs": "phpcs --standard=ruleset.xml",
  "cs:fix": "phpcbf --standard=ruleset.xml"
}
```

3. Add ruleset.xml configuration file at root level:

```xml
<?xml version="1.0"?>
<ruleset name="koel">
    <config name="installed_paths" value="../../slevomat/coding-standard"/>

    <file>./app</file>
    <file>./bootstrap</file>
    <file>./config</file>
    <file>./database</file>
    <file>./routes</file>
    <file>./tests</file>

    <arg name="extensions" value="php,inc" />
    <arg name="tab-width" value="4"/>
    <arg name="colors"/>
    <arg value="sp"/>

    <exclude-pattern>*.blade.php</exclude-pattern>
    <exclude-pattern>*/cache/*</exclude-pattern>

    <rule ref="SlevomatCodingStandard.TypeHints.ReturnTypeHint">
        <!-- exclude Controllers due to the flexible nature of controller's action methods -->
        <exclude-pattern>*Controller.php</exclude-pattern>
        <properties>
            <property name="traversableTypeHints" type="array">
                <element value="\Illuminate\Support\Collection"/>
                <element value="\Illuminate\Database\Eloquent\Collection"/>
            </property>
        </properties>
    </rule>
    <rule ref="SlevomatCodingStandard.TypeHints.UselessConstantTypeHint"/>
    <rule ref="SlevomatCodingStandard.Exceptions.ReferenceThrowableOnly"/>
    <rule ref="SlevomatCodingStandard.Arrays.DisallowImplicitArrayCreation"/>
    <rule ref="SlevomatCodingStandard.Classes.DisallowLateStaticBindingForConstants"/>
    <rule ref="SlevomatCodingStandard.Classes.UselessLateStaticBinding"/>
    <rule ref="SlevomatCodingStandard.ControlStructures.AssignmentInCondition"/>
    <rule ref="SlevomatCodingStandard.ControlStructures.DisallowContinueWithoutIntegerOperandInSwitch"/>
    <rule ref="SlevomatCodingStandard.ControlStructures.DisallowEmpty"/>
    <rule ref="SlevomatCodingStandard.ControlStructures.RequireNullCoalesceOperator"/>
    <rule ref="SlevomatCodingStandard.ControlStructures.RequireNullCoalesceEqualOperator"/>
    <rule ref="SlevomatCodingStandard.Functions.StrictCall"/>
    <rule ref="SlevomatCodingStandard.Functions.StaticClosure"/>
    <rule ref="SlevomatCodingStandard.PHP.DisallowDirectMagicInvokeCall"/>
    <rule ref="SlevomatCodingStandard.Operators.DisallowEqualOperators"/>
    <rule ref="SlevomatCodingStandard.Operators.DisallowEqualOperators"/>
    <rule ref="SlevomatCodingStandard.Functions.UnusedInheritedVariablePassedToClosure"/>
    <rule ref="SlevomatCodingStandard.Functions.UselessParameterDefaultValue"/>
    <rule ref="SlevomatCodingStandard.Namespaces.UnusedUses">
        <properties>
            <property name="searchAnnotations" value="true"/>
        </properties>
    </rule>
    <rule ref="SlevomatCodingStandard.Namespaces.UseFromSameNamespace"/>
    <rule ref="SlevomatCodingStandard.Namespaces.UselessAlias"/>
    <rule ref="SlevomatCodingStandard.PHP.UselessParentheses"/>
    <rule ref="SlevomatCodingStandard.PHP.OptimizedFunctionsWithoutUnpacking"/>
    <rule ref="SlevomatCodingStandard.PHP.UselessSemicolon"/>
    <rule ref="SlevomatCodingStandard.Variables.DisallowSuperGlobalVariable"/>
    <rule ref="SlevomatCodingStandard.Variables.DuplicateAssignmentToVariable"/>
    <rule ref="SlevomatCodingStandard.Variables.UnusedVariable"/>
    <rule ref="SlevomatCodingStandard.Variables.UselessVariable"/>
    <rule ref="SlevomatCodingStandard.Exceptions.DeadCatch"/>
    <rule ref="SlevomatCodingStandard.Arrays.MultiLineArrayEndBracketPlacement"/>
    <rule ref="SlevomatCodingStandard.Arrays.SingleLineArrayWhitespace"/>
    <rule ref="SlevomatCodingStandard.Arrays.TrailingArrayComma"/>
    <rule ref="SlevomatCodingStandard.Classes.ClassMemberSpacing"/>
    <rule ref="SlevomatCodingStandard.Classes.DisallowMultiConstantDefinition"/>
    <rule ref="SlevomatCodingStandard.Classes.DisallowMultiPropertyDefinition"/>
    <rule ref="SlevomatCodingStandard.Classes.MethodSpacing"/>
    <rule ref="SlevomatCodingStandard.Classes.ParentCallSpacing"/>
    <rule ref="SlevomatCodingStandard.Classes.PropertySpacing"/>
    <rule ref="SlevomatCodingStandard.Classes.RequireMultiLineMethodSignature"/>
    <rule ref="SlevomatCodingStandard.Classes.TraitUseDeclaration"/>
    <rule ref="SlevomatCodingStandard.ControlStructures.BlockControlStructureSpacing"/>
    <rule ref="SlevomatCodingStandard.ControlStructures.LanguageConstructWithParentheses"/>
    <rule ref="SlevomatCodingStandard.ControlStructures.NewWithParentheses"/>
    <rule ref="SlevomatCodingStandard.ControlStructures.RequireMultiLineCondition"/>
    <rule ref="SlevomatCodingStandard.ControlStructures.RequireShortTernaryOperator"/>
    <rule ref="SlevomatCodingStandard.ControlStructures.DisallowYodaComparison"/>
    <rule ref="SlevomatCodingStandard.Files.LineLength"/>
    <rule ref="SlevomatCodingStandard.Functions.ArrowFunctionDeclaration"/>
    <rule ref="SlevomatCodingStandard.Namespaces.AlphabeticallySortedUses"/>
    <rule ref="SlevomatCodingStandard.Namespaces.RequireOneNamespaceInFile"/>
    <rule ref="SlevomatCodingStandard.Namespaces.NamespaceDeclaration"/>
    <rule ref="SlevomatCodingStandard.Namespaces.NamespaceSpacing"/>
    <rule ref="SlevomatCodingStandard.TypeHints.DisallowArrayTypeHintSyntax"/>
    <rule ref="SlevomatCodingStandard.PHP.TypeCast"/>
    <rule ref="SlevomatCodingStandard.Classes.ClassConstantVisibility"/>
    <rule ref="SlevomatCodingStandard.TypeHints.ReturnTypeHintSpacing">
        <properties>
            <property name="spacesCountBeforeColon" value="0"/>
        </properties>
    </rule>
    <rule ref="SlevomatCodingStandard.TypeHints.NullableTypeForNullDefaultValue"/>
    <rule ref="SlevomatCodingStandard.TypeHints.ParameterTypeHintSpacing"/>
    <rule ref="SlevomatCodingStandard.TypeHints.PropertyTypeHintSpacing"/>
    <rule ref="SlevomatCodingStandard.Namespaces.DisallowGroupUse"/>
    <rule ref="SlevomatCodingStandard.Namespaces.MultipleUsesPerLine"/>
    <rule ref="SlevomatCodingStandard.Namespaces.ReferenceUsedNamesOnly">
        <properties>
            <property name="searchAnnotations" value="true"/>
        </properties>
    </rule>
    <rule ref="SlevomatCodingStandard.Namespaces.UseDoesNotStartWithBackslash"/>
    <rule ref="SlevomatCodingStandard.Commenting.ForbiddenAnnotations"/>
    <rule ref="SlevomatCodingStandard.Commenting.EmptyComment"/>
    <rule ref="SlevomatCodingStandard.Commenting.RequireOneLinePropertyDocComment"/>
    <rule ref="SlevomatCodingStandard.Commenting.UselessFunctionDocComment"/>
    <rule ref="SlevomatCodingStandard.Commenting.UselessInheritDocComment"/>
    <rule ref="SlevomatCodingStandard.ControlStructures.UselessIfConditionWithReturn"/>
    <rule ref="SlevomatCodingStandard.ControlStructures.UselessTernaryOperator"/>

    <rule ref="PSR1.Classes.ClassDeclaration.MissingNamespace">
        <exclude-pattern>*/database/migrations/*</exclude-pattern>
    </rule>

    <rule ref="PSR1.Files.SideEffects">
        <exclude-pattern>bootstrap/autoload.php</exclude-pattern>
    </rule>

    <rule ref="PSR2">
        <exclude name="PSR2.ControlStructures.SwitchDeclaration"/>
        <exclude name="PSR2.ControlStructures.ControlStructureSpacing.SpacingAfterOpenBrace"/>

        <!-- already handled by SlevomatCodingStandard.Files.LineLength -->
        <exclude name="Generic.Files.LineLength.TooLong"/>
    </rule>

    <rule ref="PSR12"/>
</ruleset>
```