// Contract for doing experiments related to geology
//   Author: nightingale

CONTRACT_TYPE
{
    name = FS_GeologicalStudy
    group = FieldResearch

    sortKey = 07
    genericTitle = Geological study
    genericDescription = Perform a geological study of a planet or moon.

    title = Geological study of @targetBody
    description = Geologically speaking, we know very little about @targetBody.  That's why we've applied for a research grant to do a geological study of @targetBody.  We haven't actually received the grant, but I'm sure things will work out fine.
    synopsis = Perform a study of @targetBody's geology.
    completedMessage = Great work, I hope Linus remembered to submit the grant paperwork!

    // Up to two
    maxSimultaneous = 2

    // Always offered by the Field Research Team
    agent = Field Research Team

    targetBody = @targetBody1

    // Contract rewards
    rewardFunds = Random(25000.0, 30000.0)
    rewardReputation = Random(2.0, 5.0)

    DATA
    {
        type = CelestialBody
        uniquenessCheck = CONTRACT_ACTIVE
        hidden = true

        targetBody1 = Prestige() == Trivial ? HomeWorld() : Prestige() == Significant ? @FieldResearch:l2Bodies.Where(cb => cb.HasSurface()).Random() : @FieldResearch:l3Bodies.Where(cb => cb.HasSurface()).SelectUnique()
    }

    // Hardcoded list of specific experiments
    DATA
    {
        type = List<ScienceExperiment>
        hidden = true

        experiments = [surfaceSample, seismicScan, gravityScan]
    }

    DATA
    {
        type = List<ScienceSubject>
        hidden = true

        scienceSubjectsTemp1 = AllScienceSubjectsByBodyExperiment([@targetBody], @experiments)
        scienceSubjectsTemp2 = @scienceSubjectsTemp1.Where(s => s.CollectedScience() == 0.0)
        scienceSubjectsTemp3 = @scienceSubjectsTemp2.Where(s => !s.Biome().IsKSC())
        scienceSubjectsTemp4 = @scienceSubjectsTemp3.Where(s => s.Situation() != SrfSplashed)
        scienceSubjects = @scienceSubjectsTemp4.Random(5)
    }

    DATA
    {
        type = ScienceRecoveryMethod
        hidden = true

        recoveryMethod = @targetBody.IsHomeWorld() || @targetBody.Parent().IsHomeWorld() ? Ideal : RecoverOrTransmit
    }

    REQUIREMENT
    {
        type = Expression
        expression = @/scienceSubjects.Count() == 5

        title = Must have valid experiments to run
    }


    PARAMETER
    {
        type = CollectScience

        biome = @scienceSubject.Biome()
        situation = @scienceSubject.Situation()
        experiment = @scienceSubject.Experiment()
        recoveryMethod = @/recoveryMethod

        rewardFunds = Random(15000.0, 16000.0)

        ITERATOR
        {
            type = ScienceSubject
            scienceSubject = @/scienceSubjects
        }
    }

    REQUIREMENT
    {
        type = Any

        REQUIREMENT
        {
            type = TechResearched

            part = sensorGravimeter
        }

        REQUIREMENT
        {
            type = TechResearched

            part = sensorAccelerometer
        }
    }
}
