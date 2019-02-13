Coding Guidelines for C#
================

Theses guidelines provides coding standards, but also good coding practices that we have adopted at TouchReality.

[Basic Principles (AV0000)](_pages/0000_BasicPrinciples.md)

[Class Design (AV1000)](_pages/1000_ClassDesignGuidelines.md)

[Member Design (AV1100)](_pages/1100_MemberDesignGuidelines.md)

[Miscellaneous (AV1200)](_pages/1200_MiscellaneousDesignGuidelines.md)

[Maintainability Guidelines (AV1500)](_pages/1500_MaintainabilityGuidelines.md)

[Naming Conventions (AV1700)](_pages/1700_NamingGuidelines.md)

[Layout Guidelines (AV2400)](_pages/2400_LayoutGuidelines.md)

## To define...

Performance Guidelines (AV1800)

.NET Framework Usage (AV2200)

Comments and XML Documentation (AV2300)

## Notes
At TouchReality, we are currently in an adoption phase. It mean that the current rules are not strict but are welcomed.
We ask you to read them, practice them, discuss them together.

Nothing is writen in the stone, it is a working document that is intended to evolve.

To help you in this decision, I’ve assigned a level of importance to each guideline:

![](/assets/images/1.png) Guidelines that you should never skip and should be applicable to all situations

![](/assets/images/2.png) Strongly recommended guidelines

![](/assets/images/3.png) May not be applicable in all situations

## Code Overview

The current section gives you a quick overview of our coding convention, of course it doesn't represents all the rules but gives you a quick entry point.

```csharp
using System;
using System.Diagnostics;
using System.Collections.Generic;

namespace TouchReality.BusinessObjects
{

    /// <summary>
    /// The Furniture class represents a kitchen's furniture, it also give access to all related
    /// information and all possible variations of the furniture.
    /// </summary>
	public sealed class Furniture
    {

        #region Variables

        /// <summary>The furniture provider identifier</summary>
        private string _providerId = "";

        /// <summary>The furniture provider instance</summary>
        private FurnitureProvider _provider = null;

        /// <summary>The textual furniture identifier. Related to the provider identifier</summary>
        private string _furnitureId = "";

        /// <summary>The list of sides</summary>
        private List<Side> _sides = new List<Side>();

        /// <summary>The style associated with each side</summary>
        private List<SideStyle> _sidesStyles = null;

        #endregion

        #region Constructors

        /// <summary>
        /// The default constructor
        /// </summary>
        private Furniture()
        { }

        /// <summary>
        /// The furniture constructor
        /// </summary>
        /// <param name="providerId">The provider identifier</param>
        /// <param name="furnitureId">The furniture identifier</param>
        public Furniture(string providerId, string furnitureId) : this()
        {
            // Sets the composed primary key
            _providerId = providerId;
            _furnitureId = furnitureId;
        }

        #endregion

        #region Properties

        /// <summary>
        /// Returns the furniture's provider.
        /// </summary>
        public FurnitureProvider Provider
        {
            get
            {
                // Lazy loading : load the provider on demand
                if (_provider == null)
                    _provider = FurnitureProvidersCache.Get(_providerId);

                return _provider;
            }
        }

        /// <summary>
        /// Returns the list of sides.
        /// </summary>
        public IReadOnlyList<Side> Sides
        {
            get
            {
                // Lazy loading : load the furnitures on demand
                if (_sides == null)
                    _sides = FurnitureDAL.LoadFurnitureSides(_furnitureId);

                return _sides;
            }
        }

        /// <summary>
        /// Returns the furniture placement's current state.
        /// </summary>
        public PlacementState PlacementState { get; } = new Furniture.PlacementState();

        #endregion

        #region GetSideStyle

        /// <summary>
        /// Find the style related to a side 
        /// </summary>
        /// <param name="side"></param>
        /// <returns></returns>
        public SideStyle GetSideStyle(Side side)
        {
            // If there is not style yet, resets to the default ones.
            if (_sidesStyles == null)
                ResetSidesStyles();

            // Search for the corresponding side style
            uint i = 0;
            for (uint i = 0; i < Sides.Count; i++)
            {
                if (ReferenceEquals(currentSide, Sides[i]))
                    return _sidesStyles[i];
            }

            // Unreachable code
            Debug.Assert(false, "Unexpected code reached. You're searching for a side that's not part of this furniture");
        }

        #endregion

        #region ResetSidesStyles

        /// <summary>
        /// Resets the furniture to the default sodes styles.
        /// </summary>
        public void ResetSidesStyles()
        {
            // The default styles are stored in the database, so we load them.
            _sidesStyles = FurnitureDAL.LoadFurnitureDefaultCustomization(_providerId, _furnitureId, Sides);
        }

        #endregion

        #region Inner class : PlacementState

        /// <summary>
        /// Provides information related to the furniture's placement in the room.
        /// </summary>
        public struct PlacementState
        {
            /// <summary>Does the furniture has a valid placement</summary>
            public bool IsValid = true;

            /// <summary>Does the furniture is part of an ilot</summary>
            public bool IsPartOfIlot = false;

            /// <summary>Does the furniture is against a wall</summary>
            public bool IsAgainstAWall = false;

            /// <summary>The wold position of the furniture, expressed in centimeters</summary>
            public Vector2D WorldPosition = new Vector2D();
        }

        #endregion

        #region Inner class : SideStyle

        /// <summary>
        /// Provides information related to some side's style customization.
        /// </summary>
        public class SideStyle
        {
            /// <summary>The current RAL color reference</summary>
            public RALColor Color;

            /// <summary>The current used material</summary>
            public Material Material;
        }

        #endregion

    }

}
```
