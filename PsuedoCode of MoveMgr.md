void __userpurge CMoveMgr::UpdateNormalMotion(CMoveMgr *this@<ecx>, int a2@<edi>, float fTimeDelta, float a4)
{
  double v5; // st7
  struct LTObject *v6; // eax
  ContainerCode v7; // eax
  int v8; // ebp
  ContainerCode v9; // eax
  struct LTObject *v10; // eax
  CCharacterFX *v11; // eax
  double v12; // st6
  double v13; // st3
  double v14; // st1
  double v15; // st6
  struct LTObject *v16; // eax
  double v17; // st7
  double v18; // st7
  double v19; // st6
  CCharacterFX *v20; // eax
  ModelId v21; // edi
  ModelFigureType v22; // eax
  double v23; // st7
  unsigned int v24; // ebx
  double v25; // st7
  double v26; // st7
  double v27; // st7
  double v28; // st6
  char v29; // al
  double v30; // st7
  double v31; // st7
  char v32; // al
  double v33; // st7
  double v34; // st7
  double v35; // st7
  double v36; // st6
  double v37; // st5
  double v38; // st4
  double v39; // st7
  TVector3<float> *v40; // eax
  TVector3<float> *v41; // eax
  double v42; // st7
  double v43; // st7
  signed int v44; // ecx
  double v45; // st7
  unsigned int v46; // eax
  double v47; // st7
  struct LTObject *v48; // eax
  float v49; // edx
  float v50; // eax
  struct LTObject *v51; // eax
  unsigned int v52; // edi
  PlayerState v53; // eax
  float v54; // ecx
  float v55; // eax
  double v56; // st7
  bool v57; // c0
  bool v58; // c3
  struct LTObject *v59; // eax
  struct LTObject *v60; // edx
  struct LTObject *v61; // [esp+30h] [ebp-B0h]
  TVector3<float> vAccel; // [esp+44h] [ebp-9Ch] BYREF
  TVector3<float> vJumpVolumeVel; // [esp+50h] [ebp-90h] BYREF
  float v64; // [esp+5Ch] [ebp-84h]
  float fScale; // [esp+60h] [ebp-80h]
  double fMoveAccel; // [esp+64h] [ebp-7Ch]
  float v67; // [esp+6Ch] [ebp-74h]
  float fMaxAccel; // [esp+70h] [ebp-70h]
  TVector3<float> vel; // [esp+74h] [ebp-6Ch] BYREF
  TVector3<float> vRightAccel; // [esp+80h] [ebp-60h] BYREF
  float v71; // [esp+8Ch] [ebp-54h]
  double v72; // [esp+90h] [ebp-50h]
  unsigned int bJumping; // [esp+98h] [ebp-48h]
  TVector3<float> myVel; // [esp+9Ch] [ebp-44h] BYREF
  TVector3<float> vForward; // [esp+A8h] [ebp-38h] BYREF
  float fJumpVel; // [esp+B4h] [ebp-2Ch]
  LTRotation rRot; // [esp+B8h] [ebp-28h] BYREF
  float fAccelInc; // [esp+C8h] [ebp-18h]
  TVector3<float> vZero; // [esp+CCh] [ebp-14h] BYREF
  unsigned int bFreeMovement; // [esp+D8h] [ebp-8h]
  int v81; // [esp+DCh] [ebp-4h]
  int retaddr; // [esp+E0h] [ebp+0h]

  vAccel.x = 0.0;
  vAccel.y = 0.0;
  vAccel.z = 0.0;
  ((void (__thiscall *)(ILTPhysics *, struct LTObject *, TVector3<float> *, int))g_pPhysicsLT->SetAcceleration)(
    g_pPhysicsLT,
    this->m_hObject,
    &vAccel,
    a2);
  CMoveMgr::UpdateContainerMotion(this, a4);
  CMoveMgr::UpdateOnGround(this);
  rRot.m_Quat[1] = 0.0;
  rRot.m_Quat[2] = 0.0;
  rRot.m_Quat[3] = 0.0;
  fAccelInc = 1.0;
  CPlayerMgr::GetPlayerRotation(g_pPlayerMgr, (LTRotation *)&rRot.m_Quat[1]);
  (*(void (__cdecl **)(struct LTObject *, float *))(*(_DWORD *)&g_pLTClient._Alval.std::_Allocator_base<wchar_t> + 540))(
    this->m_hObject,
    &rRot.m_Quat[1]);
  v5 = 0.0;
  myVel.y = 0.0;
  myVel.z = 0.0;
  vForward.x = 0.0;
  if ( g_pPhysicsLT )
  {
    v6 = this->m_hObject;
    if ( v6 )
    {
      g_pPhysicsLT->GetVelocity(g_pPhysicsLT, v6, (TVector3<float> *)&myVel.y);
      v5 = 0.0;
    }
  }
  v64 = vForward.x;
  v7 = this->m_eCurContainerCode;
  vJumpVolumeVel.z = v5;
  vJumpVolumeVel.y = myVel.y;
  if ( v7 > CC_NO_CONTAINER && (v7 <= CC_CORROSIVE_FLUID || v7 == CC_FREEZING_WATER)
    || v7 == CC_LADDER
    || this->m_bBodyOnLadder
    || g_pPlayerMgr->m_bSpectatorMode )
  {
    v8 = 1;
    v81 = 1;
  }
  else
  {
    v8 = 0;
    v81 = 0;
  }
  v67 = CMoveMgr::GetMaxVelMag(this);
  if ( this->m_SlowTimer.m_tp <= 0.0 )
  {
    if ( this->m_SlowTimer.m_dt > ((double (__thiscall *)(_DWORD))*(_DWORD *)(**(_DWORD **)&g_pLTClient._Alval.std::_Allocator_base<wchar_t>
                                                                            + 124))(*(_DWORD *)&g_pLTClient._Alval.std::_Allocator_base<wchar_t>)
                                - this->m_SlowTimer.m_t1 )
      goto LABEL_17;
    goto LABEL_16;
  }
  if ( this->m_SlowTimer.m_dt <= this->m_SlowTimer.m_tp - this->m_SlowTimer.m_t1 )
LABEL_16:
    this->m_fSlowSpeedByDamage = 1.0;
LABEL_17:
  myVel.x = 0.0;
  if ( v8 )
  {
    if ( (this->m_dwControlFlags & 4) != 0 )
      LODWORD(myVel.x) = 1;
    this->m_bJumpRequested = 0;
  }
  else if ( !this->m_bJumpRequested || (LODWORD(myVel.x) = 1, this->m_bJumped) )
  {
    myVel.x = 0.0;
  }
  if ( this->m_bBodyOnLadder )
  {
    if ( myVel.z > 200.0 )
    {
      myVel.z = 200.0;
      g_pPhysicsLT->SetVelocity(g_pPhysicsLT, this->m_hObject, (TVector3<float> *)&myVel.y);
    }
  }
  else
  {
    fMaxAccel = 0.0 * 0.0 + vJumpVolumeVel.y * vJumpVolumeVel.y + v64 * v64;
    fMaxAccel = sqrt(fMaxAccel);
    if ( v67 < (double)fMaxAccel )
    {
      TVector3<float>::Normalize((TVector3<float> *)&vJumpVolumeVel.y);
      v61 = this->m_hObject;
      vJumpVolumeVel.y = vJumpVolumeVel.y * v67;
      v64 = v67 * v64;
      myVel.y = vJumpVolumeVel.y;
      vForward.x = v64;
      g_pPhysicsLT->SetVelocity(g_pPhysicsLT, v61, (TVector3<float> *)&myVel.y);
    }
  }
  v9 = this->m_eLastContainerCode;
  if ( v9 > CC_NO_CONTAINER && (v9 <= CC_CORROSIVE_FLUID || v9 == CC_FREEZING_WATER) || v9 == CC_LADDER )
  {
    if ( !v8 )
    {
      myVel.y = 0.0;
      myVel.z = 0.0;
      v10 = this->m_hObject;
      vForward.x = 0.0;
      g_pPhysicsLT->SetVelocity(g_pPhysicsLT, v10, (TVector3<float> *)&myVel.y);
      goto LABEL_35;
    }
LABEL_40:
    CPlayerMgr::GetCameraRotation(g_pPlayerMgr, (LTRotation *)&rRot.m_Quat[1]);
    goto LABEL_37;
  }
  if ( v8 )
    goto LABEL_40;
LABEL_35:
  (*(void (__thiscall **)(_DWORD, struct LTObject *, float *))(**(_DWORD **)&g_pLTClient._Alval.std::_Allocator_base<wchar_t>
                                                             + 164))(
    *(_DWORD *)&g_pLTClient._Alval.std::_Allocator_base<wchar_t>,
    this->m_hObject,
    &rRot.m_Quat[1]);
  v11 = this->m_pCharFX;
  if ( v11 )
  {
    vJumpVolumeVel.y = 0.0;
    vJumpVolumeVel.z = 1.0;
    v64 = 0.0;
    LTRotation::Rotate((LTRotation *)&rRot.m_Quat[1], (TVector3<float> *)&vJumpVolumeVel.y, v11->m_cs.fYaw);
  }
LABEL_37:
  v12 = rRot.m_Quat[2] * rRot.m_Quat[2];
  vZero.y = 1.0 - (rRot.m_Quat[3] * rRot.m_Quat[3] + v12) * 2.0;
  v13 = rRot.m_Quat[3] * rRot.m_Quat[1];
  v14 = rRot.m_Quat[2] * fAccelInc;
  *(float *)&bFreeMovement = (v13 - v14) * 2.0;
  vJumpVolumeVel.y = (v13 + v14) * 2.0;
  vJumpVolumeVel.z = (rRot.m_Quat[3] * rRot.m_Quat[2] - fAccelInc * rRot.m_Quat[1]) * 2.0;
  v64 = 1.0 - 2.0 * (v12 + rRot.m_Quat[1] * rRot.m_Quat[1]);
  vForward.y = vJumpVolumeVel.y;
  vForward.z = vJumpVolumeVel.z;
  fJumpVel = v64;
  if ( g_vtBaseMoveAccel.m_bInit )
  {
    *((float *)&fMoveAccel + 1) = g_vtBaseMoveAccel.m_fVal;
    v15 = 0.0;
  }
  else
  {
    v15 = 0.0;
    *((float *)&fMoveAccel + 1) = 0.0;
  }
  if ( g_pPlayerMgr->m_bSpectatorMode )
  {
    vJumpVolumeVel.y = v15;
    vJumpVolumeVel.z = vJumpVolumeVel.y;
    v16 = this->m_hObject;
    v64 = vJumpVolumeVel.y;
    g_pPhysicsLT->SetVelocity(g_pPhysicsLT, v16, (TVector3<float> *)&vJumpVolumeVel.y);
    if ( (this->m_dwControlFlags & 0x40) != 0 )
      v17 = 600000.0;
    else
      v17 = 300000.0;
    *((float *)&fMoveAccel + 1) = v17;
    if ( this->m_CV_SpectatorSpeedMul.m_bInit )
    {
      v67 = this->m_CV_SpectatorSpeedMul.m_fVal;
      v18 = 0.0;
      v19 = v67 * *((float *)&fMoveAccel + 1);
    }
    else
    {
      v18 = 0.0;
      v67 = 0.0;
      v19 = (float)0.0 * *((float *)&fMoveAccel + 1);
    }
    *((float *)&fMoveAccel + 1) = v19;
  }
  else if ( v8 )
  {
    if ( this->m_bBodyOnLadder )
    {
      v18 = v15;
    }
    else
    {
      if ( v15 < vJumpVolumeVel.z )
        vForward.z = v15;
      TVector3<float>::Normalize((TVector3<float> *)&vForward.y);
      v18 = 0.0;
    }
  }
  else
  {
    vForward.z = v15;
    TVector3<float>::Normalize((TVector3<float> *)&vForward.y);
    v18 = 0.0;
  }
  v20 = this->m_pCharFX;
  if ( v20 )
    v21 = v20->m_cs.eModelId;
  else
    v21 = 0;
  if ( v20 )
    v22 = v20->m_cs.eFigureType;
  else
    v22 = eModelFigureTypeStandard;
  if ( g_vtJumpVel.m_bInit )
    v18 = g_vtJumpVel.m_fVal;
  rRot.m_Quat[0] = v18;
  *(float *)&fMoveAccel = CModelButeMgr::GetFigureTypeVariableJump(g_pModelButeMgr, v22);
  v23 = CModelButeMgr::GetModelVariableJump(g_pModelButeMgr, v21);
  v24 = this->m_dwControlFlags;
  *(float *)&fMoveAccel = v23 + *(float *)&fMoveAccel;
  fMaxAccel = (*(float *)&fMoveAccel + 1.0) * rRot.m_Quat[0];
  v25 = fMaxAccel;
  LODWORD(fMaxAccel) = v24 & 8;
  rRot.m_Quat[0] = v25 * this->m_fJumpDelay;
  if ( (v24 & 8) != 0 && !this->m_bBodyOnLadder )
    *((float *)&fMoveAccel + 1) = *((float *)&fMoveAccel + 1) * 0.5;
  if ( this->m_bJumped && !v8 )
  {
    if ( g_vtInAirAccelMultiplier.m_bInit )
      v26 = g_vtInAirAccelMultiplier.m_fVal;
    else
      v26 = 0.0;
    v67 = v26;
    *((float *)&fMoveAccel + 1) = v67 * *((float *)&fMoveAccel + 1);
  }
  v27 = *((float *)&fMoveAccel + 1);
  if ( g_pPlayerMgr->m_ePlayerState != PS_DYING )
  {
    vRightAccel.y = vForward.y * v27;
    vRightAccel.z = vForward.z * v27;
    v71 = fJumpVel * v27;
    vJumpVolumeVel.y = vRightAccel.y;
    vJumpVolumeVel.z = vRightAccel.z;
    v64 = v71;
    *(float *)&v72 = vZero.y * v27;
    *((float *)&v72 + 1) = 0.0 * v27;
    *(float *)&bJumping = *(float *)&bFreeMovement * v27;
    vRightAccel.y = *(float *)&v72;
    vRightAccel.z = *((float *)&v72 + 1);
    v71 = *(float *)&bJumping;
    v28 = 0.0;
    v67 = 0.0;
    if ( g_vtStartAccel.m_bInit )
      *(float *)&fMoveAccel = g_vtStartAccel.m_fVal;
    else
      *(float *)&fMoveAccel = 0.0;
    if ( g_vtMaxAccel.m_bInit )
      vel.x = g_vtMaxAccel.m_fVal;
    else
      vel.x = 0.0;
    if ( g_vtAccelInc.m_bInit )
      v28 = g_vtAccelInc.m_fVal;
    vZero.x = v28;
    if ( LODWORD(myVel.x) || fMaxAccel != 0.0 )
      goto LABEL_128;
    v29 = 0;
    v30 = *(float *)&fMoveAccel;
    if ( (v24 & 0x100) != 0 && (this->m_dwLastControlFlags & 0x100) == 0 )
    {
      v67 = *(float *)&fMoveAccel;
      v29 = 1;
      this->m_fLastForwardAccel = *(float *)&fMoveAccel;
      this->m_fLastRightAccel = v30;
    }
    retaddr = v24 & 1;
    if ( (v24 & 1) != 0 && (this->m_dwLastControlFlags & 2) != 0 )
    {
      TVector3<float>::Normalize((TVector3<float> *)&vJumpVolumeVel.y);
      TVector3<float>::operator*=((TVector3<float> *)&vJumpVolumeVel.y, *(const float *)&fMoveAccel);
      this->m_fLastForwardAccel = *(float *)&fMoveAccel;
      v29 = 1;
    }
    LODWORD(fScale) = v24 & 2;
    if ( (v24 & 2) != 0 && (this->m_dwLastControlFlags & 1) != 0 )
    {
      TVector3<float>::Normalize((TVector3<float> *)&vJumpVolumeVel.y);
      TVector3<float>::operator*=((TVector3<float> *)&vJumpVolumeVel.y, *(const float *)&fMoveAccel);
      v31 = *(float *)&fMoveAccel;
    }
    else
    {
      if ( v29 || (v24 & 1) == 0 && (v24 & 2) == 0 )
      {
LABEL_102:
        v32 = 0;
        if ( (v24 & 0x10) != 0 && (this->m_dwLastControlFlags & 0x20) != 0 )
        {
          TVector3<float>::Normalize((TVector3<float> *)&vRightAccel.y);
          TVector3<float>::operator*=((TVector3<float> *)&vRightAccel.y, *(const float *)&fMoveAccel);
          this->m_fLastRightAccel = *(float *)&fMoveAccel;
          v32 = 1;
        }
        if ( (v24 & 0x20) != 0 && (this->m_dwLastControlFlags & 0x10) != 0 )
        {
          TVector3<float>::Normalize((TVector3<float> *)&vRightAccel.y);
          TVector3<float>::operator*=((TVector3<float> *)&vRightAccel.y, *(const float *)&fMoveAccel);
          v33 = *(float *)&fMoveAccel;
        }
        else
        {
          if ( v32 || (v24 & 0x10) == 0 && (v24 & 0x20) == 0 )
            goto LABEL_115;
          v72 = vZero.x * a4 + this->m_fLastRightAccel;
          if ( vel.x <= v72 )
          {
            TVector3<float>::Normalize((TVector3<float> *)&vRightAccel.y);
            TVector3<float>::operator*=((TVector3<float> *)&vRightAccel.y, vel.x);
            v33 = vel.x;
          }
          else
          {
            TVector3<float>::Normalize((TVector3<float> *)&vRightAccel.y);
            *(float *)&v72 = v72;
            TVector3<float>::operator*=((TVector3<float> *)&vRightAccel.y, *(const float *)&v72);
            v33 = *(float *)&v72;
          }
        }
        this->m_fLastRightAccel = v33;
LABEL_115:
        if ( !retaddr && fScale == 0.0 )
        {
          if ( g_vtStartAccel.m_bInit )
            v34 = g_vtStartAccel.m_fVal;
          else
            v34 = 0.0;
          *(float *)&v72 = v34;
          this->m_fLastForwardAccel = *(float *)&v72;
        }
        if ( (v24 & 0x10) == 0 && (v24 & 0x20) == 0 )
        {
          if ( g_vtStartAccel.m_bInit )
            v35 = g_vtStartAccel.m_fVal;
          else
            v35 = 0.0;
          *(float *)&v72 = v35;
          this->m_fLastRightAccel = *(float *)&v72;
        }
        v27 = *((float *)&fMoveAccel + 1);
LABEL_128:
        v36 = v64;
        v37 = vJumpVolumeVel.z;
        v38 = vJumpVolumeVel.y;
        if ( (v24 & 1) != 0 )
        {
          vAccel.y = v38 + vAccel.y;
          vAccel.z = v37 + vAccel.z;
          vJumpVolumeVel.x = v36 + vJumpVolumeVel.x;
        }
        if ( (v24 & 2) != 0 )
        {
          vAccel.y = vAccel.y - v38;
          vAccel.z = vAccel.z - v37;
          vJumpVolumeVel.x = vJumpVolumeVel.x - v36;
        }
        if ( v81 )
        {
          if ( LODWORD(myVel.x) )
            vAccel.z = vAccel.z + v27;
          if ( fMaxAccel != 0.0 )
            vAccel.z = -v27;
        }
        if ( (v24 & 0x10) != 0 )
        {
          vAccel.y = vAccel.y - vRightAccel.y;
          vAccel.z = vAccel.z - vRightAccel.z;
          vJumpVolumeVel.x = vJumpVolumeVel.x - v71;
          fScale = vJumpVolumeVel.x * vJumpVolumeVel.x + vAccel.y * vAccel.y + vAccel.z * vAccel.z;
          fScale = sqrt(fScale);
          if ( vel.x < (double)fScale )
          {
            TVector3<float>::Normalize((TVector3<float> *)&vAccel.y);
            vAccel.y = vAccel.y * vel.x;
            vAccel.z = vAccel.z * vel.x;
            vJumpVolumeVel.x = vel.x * vJumpVolumeVel.x;
          }
        }
        if ( (v24 & 0x20) != 0 )
        {
          vAccel.y = vRightAccel.y + vAccel.y;
          vAccel.z = vRightAccel.z + vAccel.z;
          vJumpVolumeVel.x = v71 + vJumpVolumeVel.x;
          fScale = vJumpVolumeVel.x * vJumpVolumeVel.x + vAccel.y * vAccel.y + vAccel.z * vAccel.z;
          fScale = sqrt(fScale);
          if ( vel.x < (double)fScale )
          {
            TVector3<float>::Normalize((TVector3<float> *)&vAccel.y);
            vAccel.y = vAccel.y * vel.x;
            vAccel.z = vAccel.z * vel.x;
            vJumpVolumeVel.x = vel.x * vJumpVolumeVel.x;
          }
        }
        if ( v67 > 0.0 )
        {
          fScale = vJumpVolumeVel.x * vJumpVolumeVel.x + vAccel.y * vAccel.y + vAccel.z * vAccel.z;
          if ( fScale > 0.0 )
          {
            TVector3<float>::Normalize((TVector3<float> *)&vAccel.y);
            vAccel.y = vAccel.y * v67;
            vAccel.z = vAccel.z * v67;
            vJumpVolumeVel.x = v67 * vJumpVolumeVel.x;
          }
        }
        goto LABEL_146;
      }
      v72 = vZero.x * a4 + this->m_fLastForwardAccel;
      if ( vel.x <= v72 )
      {
        TVector3<float>::Normalize((TVector3<float> *)&vJumpVolumeVel.y);
        TVector3<float>::operator*=((TVector3<float> *)&vJumpVolumeVel.y, vel.x);
        v31 = vel.x;
      }
      else
      {
        TVector3<float>::Normalize((TVector3<float> *)&vJumpVolumeVel.y);
        *(float *)&v72 = v72;
        TVector3<float>::operator*=((TVector3<float> *)&vJumpVolumeVel.y, *(const float *)&v72);
        v31 = *(float *)&v72;
      }
    }
    this->m_fLastForwardAccel = v31;
    goto LABEL_102;
  }
LABEL_146:
  if ( g_pPlayerMgr->m_bSpectatorMode )
  {
    v39 = *((float *)&fMoveAccel + 1);
    if ( (v24 & 1) != 0 )
    {
      vJumpVolumeVel.y = vForward.y * *((float *)&fMoveAccel + 1);
      vJumpVolumeVel.z = vForward.z * *((float *)&fMoveAccel + 1);
      v64 = fJumpVel * *((float *)&fMoveAccel + 1);
      vAccel.y = vJumpVolumeVel.y + vAccel.y;
      vAccel.z = vJumpVolumeVel.z + vAccel.z;
      vJumpVolumeVel.x = v64 + vJumpVolumeVel.x;
    }
    if ( (v24 & 2) != 0 )
    {
      vJumpVolumeVel.y = vForward.y * v39;
      vJumpVolumeVel.z = vForward.z * v39;
      v64 = v39 * fJumpVel;
      vAccel.y = vAccel.y - vJumpVolumeVel.y;
      vAccel.z = vAccel.z - vJumpVolumeVel.z;
      vJumpVolumeVel.x = vJumpVolumeVel.x - v64;
    }
    if ( (v24 & 0x20) != 0 )
    {
      v40 = LTRotation::Right((LTRotation *)&rRot.m_Quat[1], (TVector3<float> *)&vZero.y);
      vJumpVolumeVel.y = v40->x * *((float *)&fMoveAccel + 1);
      vJumpVolumeVel.z = v40->y * *((float *)&fMoveAccel + 1);
      v64 = *((float *)&fMoveAccel + 1) * v40->z;
      vAccel.y = vJumpVolumeVel.y + vAccel.y;
      vAccel.z = vJumpVolumeVel.z + vAccel.z;
      vJumpVolumeVel.x = v64 + vJumpVolumeVel.x;
    }
    if ( (v24 & 0x10) != 0 )
    {
      v41 = LTRotation::Right((LTRotation *)&rRot.m_Quat[1], (TVector3<float> *)&vZero.y);
      vJumpVolumeVel.y = *((float *)&fMoveAccel + 1) * v41->x;
      vJumpVolumeVel.z = v41->y * *((float *)&fMoveAccel + 1);
      v64 = *((float *)&fMoveAccel + 1) * v41->z;
      vAccel.y = vAccel.y - vJumpVolumeVel.y;
      vAccel.z = vAccel.z - vJumpVolumeVel.z;
      vJumpVolumeVel.x = vJumpVolumeVel.x - v64;
    }
    TVector3<float>::Normalize((TVector3<float> *)&vAccel.y);
    fScale = this->m_fSpectModeAcc + 300000.0;
    vAccel.y = fScale * vAccel.y;
    vAccel.z = fScale * vAccel.z;
    vJumpVolumeVel.x = fScale * vJumpVolumeVel.x;
  }
  if ( this->m_fJumpDelay < 1.0 )
  {
    fScale = this->m_fJumpDelay;
    vJumpVolumeVel.y = fScale * vAccel.y;
    vJumpVolumeVel.z = fScale * vAccel.z;
    v64 = fScale * vJumpVolumeVel.x;
    vAccel.y = vJumpVolumeVel.y;
    vAccel.z = vJumpVolumeVel.z;
    vJumpVolumeVel.x = v64;
  }
  if ( this->m_DamageTimer.m_bOn )
  {
    v42 = this->m_DamageTimer.m_tp <= 0.0 ? ((double (__thiscall *)(_DWORD))*(_DWORD *)(**(_DWORD **)&g_pLTClient._Alval.std::_Allocator_base<wchar_t>
                                                                                      + 124))(*(_DWORD *)&g_pLTClient._Alval.std::_Allocator_base<wchar_t>) : this->m_DamageTimer.m_tp;
    if ( this->m_DamageTimer.m_dt > v42 - this->m_DamageTimer.m_t1 )
    {
      if ( this->m_DamageTimer.m_tp <= 0.0 )
        v43 = ((double (__thiscall *)(_DWORD))*(_DWORD *)(**(_DWORD **)&g_pLTClient._Alval.std::_Allocator_base<wchar_t>
                                                        + 124))(*(_DWORD *)&g_pLTClient._Alval.std::_Allocator_base<wchar_t>);
      else
        v43 = this->m_DamageTimer.m_tp;
      v44 = this->m_nKnockBackValue;
      fMaxAccel = v43 - this->m_DamageTimer.m_t1;
      v45 = (double)(int)this->m_nKnockBackValue;
      if ( v44 < 0 )
        v45 = v45 + 4294967300.0;
      v46 = this->m_dwControlFlags;
      fScale = 1.0 - fMaxAccel;
      *(float *)&fMoveAccel = v45 * v45 * fScale;
      if ( (v46 & 8) != 0 )
      {
        if ( (v46 & 0x100) != 0 )
        {
          v47 = *(float *)&fMoveAccel * 0.25;
        }
        else
        {
          vZero.y = 0.0;
          vZero.z = 0.0;
          v48 = this->m_hObject;
          *(float *)&bFreeMovement = 0.0;
          g_pPhysicsLT->SetVelocity(g_pPhysicsLT, v48, (TVector3<float> *)&vZero.y);
          v47 = 0.0;
        }
        *(float *)&fMoveAccel = v47;
      }
      v49 = g_pPlayerMgr->m_vDeathVec.y;
      v50 = g_pPlayerMgr->m_vDeathVec.z;
      vJumpVolumeVel.y = g_pPlayerMgr->m_vDeathVec.x;
      vAccel.y = vJumpVolumeVel.y;
      v64 = v50;
      vJumpVolumeVel.z = v49;
      vJumpVolumeVel.x = v50;
      vAccel.z = 0.0;
      fScale = v50 * v50 + vJumpVolumeVel.y * vJumpVolumeVel.y + 0.0 * 0.0;
      if ( fScale != 0.0 )
      {
        TVector3<float>::Normalize((TVector3<float> *)&vAccel.y);
        vAccel.y = vAccel.y * *(float *)&fMoveAccel;
        vAccel.z = *(float *)&fMoveAccel * vAccel.z;
        vJumpVolumeVel.x = *(float *)&fMoveAccel * vJumpVolumeVel.x;
      }
    }
  }
  g_pPhysicsLT->SetAcceleration(g_pPhysicsLT, this->m_hObject, (TVector3<float> *)&vAccel.y);
  vel.y = 0.0;
  vel.z = 0.0;
  vRightAccel.x = 0.0;
  if ( g_pPhysicsLT )
  {
    v51 = this->m_hObject;
    if ( v51 )
      ((void (__stdcall *)(struct LTObject *, float *))g_pPhysicsLT->GetVelocity)(v51, &vel.y);
  }
  v52 = 0;
  if ( !LODWORD(myVel.x)
    || g_pPlayerMgr->m_ePlayerExtState == PES_GM
    || (v53 = g_pPlayerMgr->m_ePlayerState, v53 == PS_DEAD)
    || v53 == PS_DYING
    || v53 == PS_WAITING )
  {
    v56 = 1.0;
    if ( this->m_fJumpDelay < 1.0 )
      v56 = a4 * 1.200000047683716 + this->m_fJumpDelay;
    goto LABEL_193;
  }
  if ( this->m_bOnGround && !this->m_bBodyOnLadder )
  {
    v54 = this->m_vJumpVolumeVel.x;
    v55 = this->m_vJumpVolumeVel.z;
    vJumpVolumeVel.z = this->m_vJumpVolumeVel.y;
    vJumpVolumeVel.y = v54;
    v64 = v55;
    v52 = 1;
    fScale = vJumpVolumeVel.z * vJumpVolumeVel.z + v54 * v54 + v55 * v55;
    fScale = sqrt(fScale);
    if ( fScale == 0.0 )
    {
      vel.z = rRot.m_Quat[0];
    }
    else
    {
      vel.y = vel.y + vJumpVolumeVel.y;
      vel.z = vJumpVolumeVel.z + vel.z;
      vRightAccel.x = v64 + vRightAccel.x;
      (*(void (__cdecl **)(_DWORD, const char *))(**(_DWORD **)&g_pLTClient._Alval.std::_Allocator_base<wchar_t> + 24))(
        *(_DWORD *)&g_pLTClient._Alval.std::_Allocator_base<wchar_t>,
        "Player In Jump by volume 2");
    }
    v56 = 0.30000001;
    this->m_bJumped = 1;
    v57 = this->m_fJumpDelay > 0.30000001;
    v58 = 0.30000001 == this->m_fJumpDelay;
    this->m_bJumpRequested = 0;
    if ( !v57 && !v58 )
      v56 = this->m_fJumpDelay - a4 * 0.05000000074505806;
LABEL_193:
    this->m_fJumpDelay = v56;
    goto LABEL_194;
  }
  v52 = 0;
LABEL_194:
  if ( g_pPlayerMgr->m_bSpectatorMode )
  {
    vel.y = vel.y * 0.8999999761581421;
    vel.z = vel.z * 0.8999999761581421;
    vRightAccel.x = 0.8999999761581421 * vRightAccel.x;
    fScale = vRightAccel.x * vRightAccel.x + vel.y * vel.y + vel.z * vel.z;
    fScale = sqrt(fScale);
    if ( fScale < 0.1000000014901161 )
    {
      vel.y = 0.0;
      vel.z = 0.0;
      vRightAccel.x = 0.0;
    }
  }
  v59 = this->m_hObject;
  vel.y = vel.y + this->m_vTotalCurrent.x;
  vel.z = this->m_vTotalCurrent.y + vel.z;
  vRightAccel.x = this->m_vTotalCurrent.z + vRightAccel.x;
  ((void (__thiscall *)(ILTPhysics *, struct LTObject *))g_pPhysicsLT->SetVelocity)(g_pPhysicsLT, v59);
  if ( g_pPlayerMgr->m_ePlayerState == PS_DYING || !g_pPlayerMgr->m_bAllowPlayerMovement )
  {
    vJumpVolumeVel.x = 0.0;
    vJumpVolumeVel.y = 0.0;
    v60 = this->m_hObject;
    vJumpVolumeVel.z = 0.0;
    g_pPhysicsLT->SetVelocity(g_pPhysicsLT, v60, &vJumpVolumeVel);
    g_pPhysicsLT->SetAcceleration(g_pPhysicsLT, this->m_hObject, &vJumpVolumeVel);
  }
  CMoveMgr::UpdateStartMotion(this, v52);
}
